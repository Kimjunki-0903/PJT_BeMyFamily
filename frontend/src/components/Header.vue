<template>
  <div class="header d-flex" style="position:fixed; padding:15px 50px;width:100%; box-shadow:2px 2px 3px rgba(0,0,0,0.3); background-color:#f2cc59;" v-if="isHeader">
      <v-btn class="mr-5" style="background-color:transparent;box-shadow: none;" @click.stop="drawer = !drawer">
        <i class="lg fas fa-bars" style="color:#4ba5cd;font-size:30px"></i>
      </v-btn>
      <v-navigation-drawer v-model="drawer" absolute temporary height="400" class="ml-4" style="border-radius:10px;">
        <v-list-item class="pl-0">
          <div class="ml-5 d-flex flex-row" style="font-size:20px;">
            <v-btn fab depressed small class="mr-3" v-if="isLoggedIn && !isManager" style="background-color: #4ba5cd">
              <v-icon color="white">mdi-human-greeting</v-icon>
            </v-btn>
            <p class="mb-0 mt-1 font-weight-black" v-if="isLoggedIn && !isManager">{{ profileData.nickName }}</p>
            <v-btn fab depressed small class="mr-3" v-if="isLoggedIn && isManager" style="background-color: #4ba5cd">
              <v-icon color="white">mdi-dog-side</v-icon>
            </v-btn>
            <p class="mb-0 mt-2 font-weight-black" v-if="isLoggedIn && isManager" style="font-size:13pt;">{{ $cookies.get('auth-token').mid }}</p>
          </div>


          <!-- <v-list-item-content> -->
          <v-dialog v-if="!isLoggedIn" v-model="dialog" persistent max-width="600" style="position: relative;">
            <template v-slot:activator="{ on, attrs }" v-if="!isLoggedIn">
              <v-btn style="position: absolute; top: 6px; left: 90px" dark v-bind="attrs" v-on="on" class="loginBtn">로그인</v-btn>
            </template>
            <!-- 수정 필요! 로그인 버튼 내려감 -->
            <v-card style="height:50vh;">
              <v-card-title>
                <span class="headline">Login</span>
              </v-card-title>
              <v-card-text>
                  <v-radio-group v-model="radios" :mandatory="false" row>
                    <v-radio label="user" value="radio-1"></v-radio>
                    <v-radio label="manager" value="radio-2"></v-radio>
                  </v-radio-group>
                  <!-- <v-row>
                    <v-col cols="12"> -->
                  <v-text-field v-if="radios === 'radio-1'" v-model="loginData.email" label="Email*" required class="pt-0"></v-text-field>
                  <v-text-field v-if="radios === 'radio-2'" v-model="loginData.mid" label="Center name*" required class="pt-0"></v-text-field>
                    <!-- </v-col>
                    <v-col cols="12"> -->
                  <v-text-field
                    v-model="loginData.password"
                    label="Password*"
                    type="password"
                    required
                  ></v-text-field>
                    <!-- </v-col>
                  </v-row> -->
                    <!-- <v-container fluid> -->
                    
                    <!-- </v-container> -->
                <p>* 아직 회원이 아니신가요?</p>
                <v-btn text color="primary" class="font-weight-black" @click="joinPage">회원가입</v-btn>
                <v-btn text color="primary" class="font-weight-black" @click="PWDialog = true">pw찾기</v-btn>
                
              </v-card-text>
              <v-divider></v-divider>
              <v-card-actions class="d-flex justify-end mt-1">
                <div class="mr-3">
                  <v-btn color="blue darken-1" text @click="dialog=false">Close</v-btn>
                  <v-btn color="blue darken-1" text @click="Login(radios)">Login</v-btn>
                </div>
              </v-card-actions>
            </v-card>
          <v-dialog v-model="PWDialog" max-width="300px">
              <v-card>
                <v-card-title>비밀번호 찾기</v-card-title>
                <v-card-text class="pb-0">
                  <v-text-field label="Email*" required v-model="PWEmail"></v-text-field>
                  <v-text-field label="NickName*" required v-model="PWNickName"></v-text-field>
                </v-card-text>
                <v-divider></v-divider>
                <v-card-actions>
                  <v-btn @click="findPW">완료</v-btn>
                  <v-btn @click="PWDialog = false">취소</v-btn>
                  <v-progress-circular
                    class="ml-3"
                    v-if="nowLoading"
                    indeterminate
                    color="primary"
                  ></v-progress-circular>
                </v-card-actions>
              </v-card>
          </v-dialog>
          </v-dialog>
        <!-- </v-list-item-content> -->
          <!-- <p class="mb-0" v-if="isLoggedIn && !isManager">{{ profileData.nickName }}</p>
          <p class="mb-0" v-else-if="isLoggedIn && isManager ">{{ $cookies.get('auth-token').mid }}</p> -->
          <!-- {{$store.state.profileData}} -->
          <!-- </v-list-item-content> -->
        </v-list-item>

        <v-divider></v-divider>
        <v-divider></v-divider>

        <v-list dense>
          <div class="ml-5 d-flex inline">
            <v-list-item-icon>
              <router-link v-bind:to="{name:constants.URL_TYPE.MAIN}">
                <i class="fas fa-home" style="color:#4ba5cd; font-size:30px;"></i>
              </router-link>
            </v-list-item-icon>
            <!-- 홈 글자 눌렀을때 메인으로 이동하는거 고치기 -->
            <v-list-item-title @click="goHome" style="cursor: pointer; font-weight:bold;">Home</v-list-item-title>
          </div>
          <div class="ml-5 d-flex inline">
            <v-list-item-icon style="color:#4ba5cd; cursor: pointer;">
              <i
                class="fas fa-id-card"
                v-if="isLoggedIn"
                @click="userProfile"
                style="font-size:30px"
              ></i>
            </v-list-item-icon>

            <v-list-item-title v-if="isLoggedIn" @click="userProfile" style="cursor: pointer; font-weight:bold">My page</v-list-item-title>
          </div>
          <div class="ml-5 d-flex inline" v-if="isLoggedIn && !isManager">
            <!-- like 아이콘, 글자 눌렀을 때 like 페이지로 이동 -->
            <v-list-item-icon style="cursor: pointer;">
              <i
                class="fas fa-paw"
                v-if="isLoggedIn"
                @click="userLike"
                style="color:#4ba5cd; font-size:30px"
              ></i>
            </v-list-item-icon>
            <v-list-item-title v-if="isLoggedIn" @click="userLike" style="cursor: pointer; font-weight:bold">Like</v-list-item-title>
          </div>
          <div class="ml-5 d-flex inline" v-if="isLoggedIn && !isManager">
            <!-- survey 아이콘, 글자 눌렀을 때 survey 페이지로 이동 -->
            <v-list-item-icon style="cursor: pointer;">
              <i
                class="far fa-file-alt ml-1"
                v-if="isLoggedIn"
                @click="userSurvey"
                style="color:#4ba5cd; font-size:30px"
              ></i>
            </v-list-item-icon>
            <v-list-item-title v-if="isLoggedIn" @click="userSurvey" style="cursor: pointer; font-weight:bold">Survey</v-list-item-title>
          </div>
          <div class="ml-5 d-flex inline" v-if="isLoggedIn && isManager">
            <!-- survey 아이콘, 글자 눌렀을 때 survey 페이지로 이동 -->
            <v-list-item-icon style="cursor: pointer;">
              <i
               class="fas fa-clipboard-list"
                @click="goManager"
                style="color:#4ba5cd; font-size:30px"
              ></i>
            </v-list-item-icon>
            <v-list-item-title @click="goManager" style="cursor: pointer; font-weight:bold">Manager</v-list-item-title>
          </div>
          <div class="ml-5 d-flex inline">
            <v-list-item-icon style="cursor: pointer;">
              <i
                class="fas fa-sign-out-alt ml-1"
                v-if="isLoggedIn"
                @click="Logout"
                style="color:#4ba5cd; font-size:30px"
              ></i>
            </v-list-item-icon>
            <v-list-item-title v-if="isLoggedIn" @click="Logout" style="cursor: pointer; font-weight:bold">Logout</v-list-item-title>
          </div>
        </v-list>
      </v-navigation-drawer>
    <div class="my-auto">
      <router-link :to="{name:constants.URL_TYPE.MAIN}" style="font-family: 'Recipekorea';font-size:1.6rem;font-weight:bold; color:#4ba5cd;" class="ml-0 text-decoration-none text-center ">Be My Family<i class="fas fa-paw pl-3"></i></router-link>
    </div>

    <div class="ml-auto d-flex">
      <div class="mr-3 my-auto">
        <!-- font-family: 'ImcreSoojin'; -->
        <p class="m-0 my-auto" @click="goList" style="font-family: 'Recipekorea';font-size:1rem; cursor:pointer; color:#4ba5cd;">보호소</p>
      </div>
      <div class="mr-3 my-auto">
        <p class="m-0 my-auto" @click="goLost" style="font-family: 'Recipekorea';font-size:1rem; cursor:pointer; color:#4ba5cd;">실종/보호/목격</p>
      </div>
      <div class="mr-3 my-auto">
        <p class="m-0 my-auto" @click="adoptList" style="font-family: 'Recipekorea';font-size:1rem; cursor:pointer; color:#4ba5cd;">입양후기</p>
      </div>
    </div>
  </div>
</template>

<script>

import constants from "@/lib/constants";
import axios from "axios";
import swal from 'sweetalert';

import { mapState, mapActions, mapMutations } from "vuex";

export default {
  name: "Header",
  components: {
    constants,
  },
  props: ["isHeader"],
  computed: {
    ...mapState(["loginData", "profileData", "isLoggedIn"]),
    email: {
      get() {
        return loginData.email;
      },
      set(value) {
        this.$store.commit("loginEmail", value);
      },
    },
    password: {
      get() {
        return loginData.password;
      },
      set(value) {
        this.$store.commit("loginPassword", value);
      },
    },
  },
  watch: {},
  created() {
    var login = this.$cookies.isKey("auth-token");
    if(login){
      var token = this.$cookies.get('auth-token')
      this.isLoggedInChecker(login)
      if(this.$cookies.get('auth-token').mid !== undefined){
        this.isManager = true
      }else{
        this.isManager = false
        this.find(token.email)
        console.log('프로필 ==> ' + this.profileData.nickName)
      }
    //   this.isLoggedInChecker(login)
    //   this.find(token.email)
    // }
    // this.isLoggedIn = this.$cookies.isKey("auth-token");
    // if(this.$cookies.isKey("auth-token")){
      // var token = this.$cookies.get('auth-token')\
    }
  },
  methods: {
    ...mapActions(["login", "logout", "find", "isLoggedInChecker"]),
    // ...mapMutations(["dialog=false"]),
    Login(radios) {
      this.$store.state.loginData.isManager = radios
      var paramData = {
        loginData: this.loginData,
        page: this.$route.name
      }
      this.login(paramData);
    },
    Logout() {
      this.logout(this.$route.name);
    },
    userProfile() {
      if(this.$route.name !== constants.URL_TYPE.USER.PROFILE){
        window.scrollTo(0, 0)
        this.$router.push({ name: constants.URL_TYPE.USER.PROFILE })
      }else{
        this.$router.go()
      }

    },
    goHome() {
      if(this.$route.name !== constants.URL_TYPE.MAIN){
        window.scrollTo(0, 0)
        this.$router.push({ name: constants.URL_TYPE.MAIN });
      }
    },
    userLike() {
      if(this.$route.name !== constants.URL_TYPE.USER.LIKE){
        window.scrollTo(0, 0)
        this.$router.push({ name: constants.URL_TYPE.USER.LIKE })
      }else{
        this.$router.go()
      }
    },
    userSurvey() {
      if(this.$route.name !== constants.URL_TYPE.USER.SURVEY){
        window.scrollTo(0, 0)
        this.$router.push({ name: constants.URL_TYPE.USER.SURVEY })
      }else{
        this.$router.go()
      }
    },
    goList(){
      if(this.$route.name !== constants.URL_TYPE.POST.LIST){
        window.scrollTo(0, 0)
        this.$router.push({ name: constants.URL_TYPE.POST.LIST })
      }
    },
    adoptList(){
      if(this.$route.name !== constants.URL_TYPE.ADOPTIONPOST.ADOPTLIST){
        window.scrollTo(0, 0)
        this.$router.push({ name: constants.URL_TYPE.ADOPTIONPOST.ADOPTLIST })
      }
    },
    goLost(){
      if(this.$route.name !== constants.URL_TYPE.LOST.LOSTLIST){
        window.scrollTo(0, 0)
        this.$router.push({ name: constants.URL_TYPE.LOST.LOSTLIST })
      }
    },
    goManager() {
      if(this.$route.name !== constants.URL_TYPE.USER.MANAGER){
        window.scrollTo(0, 0)
        this.$router.push( { name: constants.URL_TYPE.USER.MANAGER })
      }
    },
    findPW(){
      this.nowLoading = true
      var formData = new FormData()
      formData.append('email', this.PWEmail)
      formData.append('uid', this.PWNickName)
      axios.post(constants.SERVER_URL + '/account/findpassword', formData
      ).then(response => {
        if(response.data.data === 'success'){
          this.PWDialog = false
          swal({
            title:'이메일로 임시 비밀번호가 발송 되었습니다.',
            icon: "success",
            button: "OK"
            })
        }else{
          swal({
            title:'비밀번호 찾기에 실패하였습니다.',
            icon: "warning",
            button: "OK"
            })
        }
        this.nowLoading = false
      }).catch(error => {
        console.log(error)
      })
    },
    joinPage(){
      this.$router.push({name:constants.URL_TYPE.USER.JOIN})
      
    }
  },

  data: function () {
    return {
      constants,
      drawer: null,
      radios: 'radio-1',
      isManager: false,
      PWDialog: false,
      PWEmail: '',
      PWNickName: '',
      nowLoading: false,
      dialog: false,
    };
  },
}

</script>

<style scoped>
            @font-face {
    font-family: 'Recipekorea';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/Recipekorea.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
            
            @font-face {
    font-family: 'ImcreSoojin';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_20-04@2.3/ImcreSoojin.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
            
@font-face {
    font-family: 'TmoneyRoundWindExtraBold';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_20-07@1.0/TmoneyRoundWindExtraBold.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
      
  @font-face {
    font-family: 'GmarketSansBold';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansBold.woff') format('woff');
    font-weight: normal;
    font-style: normal;
  }
            
.header{
  /* height: 80px; */
  z-index: 11;
}
/* .hambuger{
  top: 50px;
  left: 50px;
} */
.v-navigation-drawer {
  margin-top: 50px;
  top: 50px;
  left: 50px;
}
.loginBtn {
  position: relative;
  top: 50px;
}
.v-list-item-icon {
  display: flex;
  cursor: pointer;
}
.v-list-item-title {
  display: flex;
  cursor: pointer;
}
/* .v-btn{
  transform: translateY(-30px);
} */
</style>
