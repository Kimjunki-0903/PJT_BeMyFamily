<template>
  <div class="mt-5 ml-5 pt-5" >
    <v-container>
      <div class="d-flex mb-5" style="margin-top:4rem;">
      <h1 class="ml-3"><v-icon x-large>mdi-book-open-variant</v-icon> 설문을 작성해주세요.</h1>
      </div> 
      <form>
        <h2 class="my-5"> <v-icon large>mdi-pencil-outline</v-icon> 인적사항 </h2>
        1. 신청인 성명: 
        <v-text-field
          v-model="profileData.name"
        ></v-text-field>
        2. 성별: 
        <v-text-field
          v-model="profileData.sex"
        ></v-text-field>
        3. 연령:
        <v-text-field
          v-model="profileData.birthdate"
        ></v-text-field>
        4. 전화번호:
        <v-text-field
          v-model="profileData.phone"
        ></v-text-field>
        5. Email:
        <v-text-field
          v-model="profileData.email"
          ></v-text-field>
        6. 결혼여부:
        <v-text-field
          v-model="profileData.marriaged"
        ></v-text-field>
        7. 신청인 직업:
        <v-text-field
          v-model="profileData.job"
          style="margin-bottom:4rem;"
        ></v-text-field>

        <h2 class="mb-5"> <v-icon large>mdi-pencil-outline</v-icon> 설문 </h2>
        1. 국적
        <v-radio-group  v-model="survey.nation">
          <div>
            <v-row class="justify-content-start">
            <v-radio class="ml-2"
              label="한국"
              value="korea"
              color="success"
            ></v-radio>
            <v-radio  class="mb-2 ml-4"
              label="외국"
              value="foreign"
              color="success"
            ></v-radio>
            </v-row>
          </div>
        </v-radio-group>
        2. 거주 지역
        <v-container fluid>
          <v-row align="center">
            <v-col cols="12" sm="2">
              <v-subheader v-text="'시/도'"></v-subheader>
            </v-col>
            <v-col cols="12" sm="4">
              <v-select
                v-model="survey.sido"
                :items="sido_states"
                :menu-props="{ maxHeight: '400' }"
                label="Select"
                hint="시/도를 선택하세요"
                persistent-hint
              ></v-select>
            </v-col>

            <v-col cols="12" sm="2">
              <v-subheader v-text="'지역'"></v-subheader>
            </v-col>

            <v-col cols="12" sm="4">
              <v-select
                v-model="survey.gugun"
                :items="gugun_states[survey.sido]"
                label="Select"
                hint="거주 지역을 선택하세요"
                persistent-hint
              ></v-select>
            </v-col>
          </v-row>
        </v-container>

        3. 강아지 키울 장소
        <v-radio-group  v-model="survey.place">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="가정 집 안"
                value="home"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="마당(펜스 O)"
                value="garden_o"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="마당(펜스 X)"
                value="garden_x"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="그 외 사업장"
                value="business"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>

        4. 이전에 반려견을 키운 경험이 있습니까?
        <v-radio-group  v-model="survey.beforeover">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="1"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="0"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>

        5. 유기견을 입양한 경험이 있습니까?
        <v-radio-group  v-model="survey.beforeadopt">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="1"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="0"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>

        6. 현재 집에서 키우고 있는 동물은 몇 마리 입니까?
        <div class="d-flex justify-content-start" style="margin-top:16px;">
          <v-col cols="12" sm="4">
            <v-text-field
              v-model="survey.presentanimal"
              outlined
              clearable
            ></v-text-field>
          </v-col>
          <v-subheader v-text="'마리'" style="padding-left:1px; margin-top:16px"></v-subheader>
        </div>

        7. 가족 구성원
        <v-form>
          <v-container>
            <v-row>
              <v-col cols="12" sm="4">
                <v-subheader v-text="'3세 미만'" style="padding-left:1px;"></v-subheader>
                <v-text-field
                  v-model="survey.familyunder"
                  label="몇 명"
                  outlined
                  clearable
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="1"></v-col>

              <v-col cols="12" sm="4">
                <v-subheader v-text="'3세이상 ~ 8살 미만'" style="padding-left:1px;"></v-subheader>
                <v-text-field
                  v-model="survey.familymiddle"
                  label="몇 명"
                  outlined
                  clearable
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-form>

      8. 모든 가족들이 동의 하였습니까?
        <v-radio-group  v-model="survey.familyagree">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="1"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="0"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>

        9. 동물 파양 횟수 
        <v-radio-group  v-model="survey.dissolution">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="1회"
                :value="1"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="2회 이상"
                :value="2"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="없음"
                :value="0"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>



      10. 알레르기 및 우울증?
        <v-radio-group  v-model="survey.sickness">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="1"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="0"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>
      
      11. 집에 사람이 없는 시간
        <div class="d-flex justify-content-start" style="margin-top:16px;">
          <v-col cols="12" sm="4">
            <v-text-field
              v-model="survey.alone"
              outlined
              clearable
            ></v-text-field>
          </v-col>
        <v-subheader v-text="'시간'" style="padding-left:1px; margin-top:16px"></v-subheader>
        </div>

      12. 장기 외출 시 반려견을 보호해 줄 장소 
        <v-radio-group  v-model="survey.temp">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="가족"
                value="family"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="호텔"
                value="hotel"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="지인"
                value="friend"
                color="success"
              ></v-radio>
                <v-radio  class="mb-2 ml-4"
                label="없음"
                value="0"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>

        13. 거주형태 
        <v-radio-group  v-model="survey.house">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="원룸"
                value="room"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="주택"
                value="housing"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아파트"
                value="apt"
                color="success"
              ></v-radio>
                <v-radio  class="mb-2 ml-4"
                label="다세대/빌라"
                value="villa"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>

      14. 반려견 한달 양육 비용
        <v-form>
          <v-container>
            <v-row>

              <v-col cols="12" sm="4">
              <!-- <v-subheader v-text="'식비:'" style="padding-left:1px;"></v-subheader> -->
                <v-text-field
                  v-model="survey.eatmoney"
                  label="식비"
                  outlined
                  clearable
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="1"></v-col>

              <v-col cols="12" sm="4">
              <!-- <v-subheader v-text="'관리 비용:'" style="padding-left:1px;"></v-subheader> -->
                <v-text-field
                  v-model="survey.caremoney"
                  label="관리 비용"
                  outlined
                  clearable
                ></v-text-field>
              </v-col>

            </v-row>
          </v-container>
        </v-form>

        <h2 class="mb-5"> <v-icon large>mdi-pencil-outline</v-icon> 동의 서약 </h2>
        1. 반려견과 평생을 함께 하실 수 있으십니까? 
        <v-radio-group v-model="agreement_1">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="yes"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="no"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>
        
        2. 양육할 여견이 되지 않을 시 보호소로 돌려 보내실 것에 동의합니까?
        <v-radio-group v-model="agreement_2">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="yes"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="no"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>
        
        3. 마이크로 칩 / 중성화 수술 / 입양비에 동의 하십니까?
        <v-radio-group v-model="agreement_3">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="yes"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="no"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>
        
        4. 가정 방문에 동의 하십니까? 
        <v-radio-group v-model="agreement_4">
          <div>
            <v-row class="justify-content-start">
              <v-radio class="ml-2"
                label="예"
                :value="yes"
                color="success"
              ></v-radio>
              <v-radio  class="mb-2 ml-4"
                label="아니오"
                :value="no"
                color="success"
              ></v-radio>
            </v-row>
          </div>
        </v-radio-group>

        <h2 class="mb-5"> <v-icon large>mdi-pencil-outline</v-icon> 주관식 </h2>
        1. 입양을 원하시는 이유는 무엇인가요? 
        <v-textarea label="구체적으로 적어주세요" v-model="survey.reason"></v-textarea>
        2. 입양동물이 짖음, 배변실수, 분리불안, 어지럽히기, 기존강아지와의 트러블을 일으킬 수 있습니다. 
          이점에 대해서 어떻게 생각하시나요? 
        <v-textarea label="구체적으로 적어주세요" v-model="survey.think"></v-textarea>

        <div class="d-flex align-center flex-column">
          <v-checkbox
            v-model="checkbox"
            label="작성 내용에 따라 입양이 불가할 수도 있습니다. 동의하시겠습니까?"
          ></v-checkbox>
          <div class="d-flex flex-row">
            <v-btn class="mr-4" @click="submit" large color="primary">저장</v-btn>
            <v-btn @click="goHome" large color="primary">홈으로</v-btn>
          </div>
        </div>
      </form>
    </v-container>
  </div>
</template>

<script>
import constants from "../../lib/constants";
import SERVER from "@/lib/constants";
import axios from "axios";
import { mapState, mapMutations, mapActions } from 'vuex'
import VueJwtDecode from 'vue-jwt-decode'
import swal from 'sweetalert';

export default {
    name: "survey",
    created(){
      var token = this.$cookies.get('auth-token')
      this.find(token.email)
    },
    mounted() {
      setTimeout(() => {
        axios.get(constants.SERVER_URL + "/care/survey?uid="+this.profileData.nickName)
        .then((response) =>{
          if(response.data.data !== 'uid not exist'){
            this.survey = response.data.object
          }
        })
      }, 100);
    },
    computed: {
      ...mapState(['profileData', 'loginData', 'sido_states', 'gugun_states']),

    },
    methods: {
      ...mapActions(['find', 'getProfile']),
        submit(){
          if(this.checkbox && this.agreement_1 && this.agreement_2 && this.agreement_3 && this.agreement_4) {
            this.survey.uid = this.profileData.nickName
            this.survey.name = this.profileData.name
            this.survey.email = this.profileData.email
            this.survey.phone = this.profileData.phone
            this.survey.job = this.profileData.job
            this.survey.marriaged = this.profileData.marriaged === '기혼' ? 1:0
            this.survey.sex = this.profileData.sex === '여자' ? 1:0
            this.survey.birthdate = this.profileData.birthdate
            axios
              .post(constants.SERVER_URL + "/care/surveyAdd", this.survey)
              .then((response) =>{
              }).catch((error) =>{
                console.log(error)
              })
              swal({
                title:'저장되었습니다!',
                icon: "success",
                button: "OK"
              })
          }else{
              swal({
                title:'동의해주세요!',
                icon: "warning",
                button: "OK"
              })
          }
        },
        getProfileData(){
          this.getProfile()
        },
        goHome(){
          window.scrollTo(0, 0);
          this.$router.push({name: constants.URL_TYPE.MAIN})
        }
    },
    data(){
        return {
          token:'',
          survey:{
            nation:"korea",
            place:"home",
            beforeover:1,
            beforeadopt:1,
            presentanimal:"",
            familyunder:"",
            familymiddle:"",
            familyagree:1,
            dissolution:0,
            sickness:0,
            alone:"",
            temp:"family",
            house:"room",
            eatmoney:"",
            caremoney:"",
            reason:"",
            think:"", 
            uid:"",
            name:"",
            sex:"",
            birthdate:"",
            phone:"",
            email:"",
            marriaged:"",
            job:"",
            sido: "",
            gugun: "",
          },
          checkbox:"",
          agreement_1: false,
          agreement_2: false,
          agreement_3: false,
          agreement_4: false,
          yes: true,
          no: false,
        }
    },
}
</script>

<style>



</style>