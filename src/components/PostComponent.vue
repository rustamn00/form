<template lang="">
    <div>
        <form @submit="postData" method="post">
            <label for="fullName">Name</label>
            <input type="text" name="fullName" id="fullName" v-model="user.fullName" required>

            <label for="nickname">Nickname</label>
            <input type="text" name="nickname" id="nickname" v-model="nickname" required>
            <span style="color:red;" v-if="msg.nickname">{{msg.nickname}}</span>

            <label for="password">Password</label>
            <input type="password" name="password" id="password" v-model="password" required>
            <span style="color:red;" v-if="msg.password">{{msg.password}}</span>

            <label for="repeatPassword">Repeat Password</label>
            <input type="password" name="repeatPassword" id="repeatPassword" v-model="repeatPassword" required>
            <span style="color:red;" v-if="msg.repeatPassword">{{msg.repeatPassword}}</span>

            <label for="phone">Phone</label>
            <input type="tel" name="phone" id="phone" v-model="phone" required>
            <span style="color:red;" v-if="msg.phone">{{msg.phone}}</span>

            <label for="gender">Gender</label>
            <select name="gender" id="gender" v-model="user.gender">
                <option value="male">Male</option>
                <option value="female">Female</option>
            </select>
            <br>
            <label for="country">Country</label>
            <input type="text" name="country" id="country" v-model="user.country">

            <label for="city">City</label>
            <input type="text" name="city" id="city" v-model="user.city">
            
            <button type="submit">Post</button>
        </form>
    </div>
</template>
<script>
// import Vue from 'vue'
import axios from 'axios'
// import VueAxios from 'vue-axios'
// Vue.use(VueAxios,axios)
export default {
    name:"PostComponent",
    data() {
        return {
            user:{
                fullName:null,
                nickname:null,
                password:null,
                repeatPassword:null,
                phone:null,
                gender:null,
                country:null,
                city:null,
            },
            msg:{},
            nickname:null,
            password:null,
            repeatPassword:null,
            phone:null,
            otp: 654321
        }
    },
    watch: {
        nickname(nickname){
            this.user.nickname = nickname;
            this.validateNickName(nickname);
        },
        password(password){
            this.user.password = password;
            this.validatePassword(password);
        },
        repeatPassword(repeatPassword){
            this.user.repeatPassword = repeatPassword;
            this.checkPassword(repeatPassword);
        },
        phone(phone){
            this.user.phone = phone;
            this.validatePhone(phone);
        }
  },
    methods:{
        postData(e) {
            console.log(this.user);
            axios.post("https://test.ok.paymo.uz/public/user/register", this.user)
                .then((result)=>{
                    this.check(result.data);
                    console.log(result);
                })
            e.preventDefault();
        },
        check(id){
            axios.post(`https://test.ok.paymo.uz/public/user/confirm-registration/${id}`, this.otp)
            .then((result) => {
                console.log(result);
            })
        },
        validateNickName(nickname) {
            if (/^([a-zA-z0-9]|[-_.]){5,}$/.test(nickname))
            {
                this.msg['nickname'] = '';
            } else{
                this.msg.nickname = 'Invalid NickName';
                if(this.msg.nickname) {
                    console.log(this.msg.nickname);
                }
            } 
        },
        validatePassword(password) {
            if (/^([a-zA-z0-9-_.+=@$!?]){8,}$/.test(password))
            {
                this.msg['password'] = '';
            } else{
                this.msg.password = 'Invalid Password';
                if(this.msg.password) {
                    console.log(this.msg.password);
                }
            } 
        },
        checkPassword(repeatPassword) {
            if (repeatPassword==this.password)
            {
                this.msg['repeatPassword'] = '';
            } else{
                this.msg.repeatPassword = 'Invalid Repeat Password';
                if(this.msg.repeatPassword) {
                    console.log(this.msg.repeatPassword);
                }
            } 
        },
        validatePhone(phone) {
            if (/^998\)[0-9]+$/.test(phone))
            {
                this.msg['phone'] = '';
            } else{
                this.msg.phone = 'Invalid Phone';
                if(this.msg.phone) {
                    console.log(this.msg.phone);
                }
            } 
        }
    }
}
</script>
<style>
form{
    display: inline-grid;
}
</style>