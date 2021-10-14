<template>
  <div class="userInput container">
          <h2>Paste the Url to be shortened</h2>
          <input type="text" required v-model="url" placeholder="Enter your link..." id="userInput" @focus="clearText">
          <button type="btn" class="btn" @click="getShortenUrl">Shorten Url</button>
  </div>
  <div>
        <!--result-->
        <h2>Shorten URL</h2>
        <p>Copy the shortened link and share it in messages, texts, posts, websites and other locations.</p>
        <div>
            <b class="error">{{errorMessage}}</b>
            <div v-if="errorMessage.length == 0">
            <h3>Your shortend link : <span id="myurl">{{shortenUrl}}</span></h3>
            <button type="btn" data-clipboard-target="#myurl" class="btn btn-copy">Copy</button>
            </div>
        </div>
    </div>
</template>

<script>
import ClipboardJS from 'clipboard'; //copy text to clipboard
export default {
    name: 'Shortener',
    created() {
        //request api
        fetch('https://api-ssl.bitly.com/v4/groups', {
            method: "GET",
            headers: {
                "Authorization": `Bearer ${this.token}`,
                "Content-Type": "application/json" 
            }
        })
        .then(response => response.json())
        .then(data => this.groupID = data.groups[0].guid);

        //clioboard
        new ClipboardJS('.btn-copy');
    },
    data() {
        return {
            url: '',
            shortenUrl: '',
            token: '0ac932367989d5f6ecf20ebb390e9cdaa29cd826',
            groupID: '',
            errorMessage: '',
            historyUrl: []
        }
    },
    methods: {
        clearText() {
            this.url = '';
            this.errorMessage = '';
        },
        getShortenUrl() {
            if(this.url.length === 0) {
               return;
            }
            const options = { //reference bit.ly api
                method: "POST",
                headers: { 
                    "Authorization": `Bearer ${this.token}`,
                    "Content-Type": "application/json" 
                },
                body: JSON.stringify({ "long_url": this.url, "domain": "bit.ly", "group_guid": this.groupID })
            }
            fetch('https://api-ssl.bitly.com/v4/shorten', options)
            .then(response => response.json())
            .then(data => {
                if(data.message === "INVALID_ARG_LONG_URL") {
                    this.errorMessage = "Invalid URL to shorten !";
                    return;
                }
                
                const {link, long_url} = data;
                this.historyUrl.push({
                    id: (new Date()).getTime(),
                    link,
                    longUrl: long_url,
                })
                
                this.shortenUrl = link;
                this.isActive = true;
                this.url = '';
                console.log(link)
            })
            .catch(error => {
                console.error('There was an error!', error);
            });
        }
    }
}

</script>

<style>
    .userInput{
        background-color: #e5e5e5;
        padding: 30px;
    }
    #userInput{
        width: 29%;
        height: 30%;
        padding: 15px;
        border: 0px;
        border-radius: 10px;
    }
    .btn{
        margin-left: 10px;
        width: 100px;
        height: 40px;
        border: 1px solid gainsboro;
        border-top-right-radius:10px ;
        border-bottom-left-radius:10px ;
        border-bottom-right-radius: 10px;
        border-top-left-radius: 10px;
        background-color: #0e8dee;
        color:white;
        
    }
    .error{
        color:tomato;
    }
    #myurl{
        color:green;
    }
    .btn:hover{
        cursor: pointer;
        border:2px solid whitesmoke;
    }
    .btn:active{
        color:green;
    }
</style>