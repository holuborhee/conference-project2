<template>
    <div> 
    <header>
        <router-link class="link" to='/'>LOMBACK</router-link>
    </header>
    <div class="list">
        <div>
            <h1>Guest here</h1>
            <ul v-for="guest in guestList" v-bind:key="guest.id">
                <div class="guest">
                    <li><h3>Name: {{guest.FullName}}</h3></li>
                    <li><h3>Phone_no: {{guest.Phone_No}}</h3></li>
                    <li><h3>email: {{guest.Email}} </h3></li>
                    <button class="button" @click="addToSpeakers(guest)"> Add To Guests</button>
                </div>
            </ul>
        </div> 
        <div>
            <h1>SPEAKERS</h1>
             <ul v-for="speaker in speakerList" v-bind:key="speaker.id">
                <div class="guest">
                    <li><h3>Name:   {{speaker.FullName}}</h3></li>
                    <li><h3>Phone_no: {{speaker.Phone_No}}</h3></li>
                    <li><h3>email:  {{speaker.Email}} </h3></li>
                    <button class="button" @click="deleteGuest(speaker)"> Remove Speaker</button>
                </div>
            </ul>
        </div>
    </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    data(){
        return {
            guestList:[],
            speakerList: []
        }
    },
    methods:{
        sortingRegistered(result){
            console.log(result)
            let guest = [];
            let speaker = []
                result.forEach(function (user) {
                    console.log(user.category)
                    if(user.category=="guest"){
                        guest.push(user);
                        console.log(guest)
                    } else{
                        speaker.push(user)
                    }
            });
            this.guestList = guest
            this.speakerList = speaker
        },

        axiosCall(){
            axios.get(`http://localhost:3000/v1/admin`)
            .then(response => {
            // JSON responses are automatically parsed.
            let result = response.data
            this.sortingRegistered(result)
            })
            .catch(e => {
            // this.errors.push(e)
            })
        },

        addToSpeakers(guest){
             axios.put(`http://localhost:3000/v1/admin/${guest.ID}`)
            .then(response => {
            // JSON responses are automatically parsed.
            let result = response.data
            if(result){
            this.axiosCall()
            }
            })
            .catch(e => {
                console.log(e)
            })
        },

        deleteGuest(speaker){
             axios.delete(`http://localhost:3000/v1/admin/${speaker.ID}`)
            .then(response => {
            // JSON responses are automatically parsed.
            let result = response.data
            if(result){
            this.axiosCall()
            }
            })
            .catch(e => {
                console.log(e)
            })
        }
    },
    created() {
        this.axiosCall()
    }
}
</script>
<style>
header{
  display: flex;
  justify-content: space-between;
  font-weight: 600;
  padding: 10px;
  background:  #009cfe;
}
 header>.admin{
  color:  #009cfe;
  padding: 5px 30px;
  background: white;
  text-align: center;
}

 header a {
  color: white;
  text-decoration: none;
  text-transform: uppercase;
  display: block;
}
.list{
    display: flex;
    justify-content: space-between;
    width: 100%;
    padding: 30px 20px;
    text-align: left;
    background: #f0f0f0;
}

.list>div{
    width: 40%;
    background: white;
    padding: 5px 10px;
}
ul{
    list-style: none;
    padding: 0;
    margin: 0;
}
.guest{
    border-bottom: 1px solid #009cfe;
    padding-bottom: 10px;
}
.button{
 border: none;
 background: #009cfe;
 color: white;
 font-weight: 600;
 padding: 5px 20px;
}
</style>
