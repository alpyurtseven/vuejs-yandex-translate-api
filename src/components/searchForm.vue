<template>
      <form @submit.prevent="translateIt()" class="well">
                <textarea v-model="searchText" cols="30" rows="5" class="form-control" placeholder="Çevirmek istediğiniz kelime/cümle yazınız."></textarea>
                
                <select class="form-control" v-model="slectedLang">
                    <option v-for="(lang,key) in languages" :value="key" >{{lang}}</option>
                </select>
                <br>
                <div class="text-left">
                    <strong>Çevrilecek Dil : </strong> {{languages[slectedLang]}}
                </div>
                <br>
                <button type="submit" class="btn btn-primary btn-block">Çevir Gelsin!</button>
            </form>
</template>

<script>
import axios from "axios"
export default {
    data(){
        return{
            searchText : "",
            languages : {},
            slectedLang : ""
        }
    },
    methods:{
        translateIt(){
         
            let url = "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20190725T211849Z.0490e1bb272e0bfe.76bb6204822f0d4e0f3e991e828d3f07f635625f&text="+ this.searchText + "&lang=" +this.slectedLang
            axios.get(url)
            .then(response => {
                  
                
                    let translatePack = {
                        fromLang : this.languages[response.data.lang.split("-")[0]],
                        toLang :this.languages[this.slectedLang],
                        fromText : this.searchText,
                        toText : response.data.text[0],
                    }
                    this.$emit('translatedEvent',translatePack)
                    })
            .catch(e => console.log(e))
        }
    },
    created(){
        axios.get("https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20190725T211849Z.0490e1bb272e0bfe.76bb6204822f0d4e0f3e991e828d3f07f635625f&ui=tr")
        .then(response => {
            this.languages=response.data.langs
        })
    }
}
</script>

<style>
.well textarea{
    background-color: wheat;
    border :2px solid royalblue
}
.form-control{
    background-color: wheat;
    border :2px solid royalblue
}
</style>
