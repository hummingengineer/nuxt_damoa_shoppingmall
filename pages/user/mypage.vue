<template>
  <v-layout justify-center align-center>
    <v-flex xs12 sm8 md6>
      <v-card class="mx-auto">
        <v-container>
          <div class="text-center"><v-avatar size="200"><v-img :src="userInfo.userAvatar"></v-img></v-avatar></div>
          <v-list>

            <v-list-item>
              <v-list-item-icon><v-icon>mdi-account</v-icon></v-list-item-icon>
              <v-list-item-content><v-list-item-title>{{ userInfo.userId }}</v-list-item-title></v-list-item-content>
              <v-list-item-action>
                <v-dialog v-model="userAvatarDialog" persistent max-width="600px">
                  <template v-slot:activator="{ on }">
                    <v-btn icon v-on="on"><v-icon>mdi-camera</v-icon></v-btn>
                  </template>
                  <v-container>
                    <v-card>
                      <v-card-title><span class="headline">프로필 사진 변경</span></v-card-title>
                      <v-card-text>
                        <v-row>
                          <v-col>
                            <v-file-input v-model="userAvatarInput" accept="image/*" label="프로필 사진 업로드" filled prepend-icon="mdi-camera"></v-file-input>
                          </v-col>
                        </v-row>
                      </v-card-text>
                      <v-card-actions>
                        <div class="flex-grow-1"></div>
                        <v-btn color="blue darken-1" text @click="userAvatarDialogClose">취소</v-btn>
                        <v-btn color="blue darken-1" text @click="userAvatarSubmit(userAvatarInput)">변경</v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-container>
                </v-dialog>
              </v-list-item-action>
            </v-list-item>
            <v-divider />

            <v-list-item>
              <v-list-item-icon><v-icon>mdi-lock</v-icon></v-list-item-icon>
              <v-list-item-content><v-list-item-title>비밀번호</v-list-item-title></v-list-item-content>
              <v-list-item-action>
                <v-dialog v-model="userPwdDialog" persistent max-width="600px">
                  <template v-slot:activator="{ on }">
                    <v-btn icon v-on="on"><v-icon>mdi-pencil</v-icon></v-btn>
                  </template>
                  <v-container>
                    <v-card>
                      <v-card-title><span class="headline">비밀번호 변경</span></v-card-title>
                      <v-card-text>
                        <v-row>
                          <v-col>
                            <v-text-field v-model="userPwdInput" label="변경할 비밀번호" outlined></v-text-field>
                          </v-col>
                        </v-row>
                      </v-card-text>
                      <v-card-actions>
                        <div class="flex-grow-1"></div>
                        <v-btn color="blue darken-1" text @click="userPwdDialogClose">취소</v-btn>
                        <v-btn color="blue darken-1" text @click="userPwdSubmit(userPwdInput)">변경</v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-container>
                </v-dialog>
              </v-list-item-action>
            </v-list-item>
            <v-divider />

            <v-list-item>
              <v-list-item-icon><v-icon>mdi-coin</v-icon></v-list-item-icon>
              <v-list-item-content><v-list-item-title>{{ commaUserMoney(userInfo.userMoney) }}</v-list-item-title></v-list-item-content>
              <v-list-item-action>
                <v-dialog v-model="userMoneyDialog" persistent max-width="600px">
                  <template v-slot:activator="{ on }">
                    <v-btn icon v-on="on"><v-icon>mdi-pencil</v-icon></v-btn>
                  </template>
                  <v-container>
                    <v-card>
                      <v-card-title><span class="headline">돈 충전</span></v-card-title>
                      <v-card-text>
                        <v-row>
                          <v-col>
                            <v-text-field v-model="userMoneyInput" label="충전할 돈" outlined></v-text-field>
                          </v-col>
                        </v-row>
                      </v-card-text>
                      <v-card-actions>
                        <div class="flex-grow-1"></div>
                        <v-btn color="blue darken-1" text @click="userMoneyDialogClose">취소</v-btn>
                        <v-btn color="blue darken-1" text @click="userMoneySubmit(userMoneyInput)">충전</v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-container>
                </v-dialog>
              </v-list-item-action>
            </v-list-item>
            <v-divider />

            <v-list-item>
              <v-row justify="center">
                <v-dialog v-model="deleteAccountDialog" persistent max-width="600px">
                  <template v-slot:activator="{ on }">
                    <v-btn color="red lighten-2" v-on="on">회원탈퇴</v-btn>
                  </template>
                  <v-card>
                    <v-card-title><span class="headline">회원탈퇴</span></v-card-title>
                    <v-card-text>회원탈퇴 하시겠습니까?</v-card-text>
                    <v-divider />
                    <v-card-actions>
                      <div class="flex-grow-1"></div>
                      <v-btn color="blue darken-1" text @click="deleteAccountDialogClose">취소</v-btn>
                      <v-btn color="blue darken-1" text @click="deleteAccountSubmit">탈퇴</v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </v-row>
            </v-list-item>
            <v-divider />

          </v-list>
        </v-container>
      </v-card>
      <v-fab-transition>
        <v-btn bottom right fab fixed @click="changeToAdmin"><v-icon>mdi-shield-account</v-icon></v-btn>
      </v-fab-transition>
    </v-flex>
  </v-layout>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  middleware: 'pageGuard',

  async asyncData ({ params, req, $axios, redirect }) {
    let { status } = await $axios.get('/api/user/session-administrator')
    if(status === 200) return redirect('/admin')
    let { data } = await $axios.get('/api/user/session-userinfo')
    return { userInfo: data }
  },

  data: () => ({
    userAvatarInput: null,  // File type
    userAvatarDialog: false,
    userPwdInput: null,
    userPwdDialog: false,
    userMoneyInput: null,
    userMoneyDialog: false,
    deleteAccountDialog: false,
  }),

  methods: {
    async userAvatarSubmit(userAvatarInput){
      if(!userAvatarInput) alert('사진을 첨부해 주세요')
      else{
        let formData = new FormData()
        formData.append('userId', this.userInfo.userId)
        formData.append('userAvatar', userAvatarInput, userAvatarInput.name)

        let { status } = await this.$axios.put('/api/user/change-avatar', formData)
        if(status === 200) { this.userAvatarInput = null; this.userAvatarDialog = false; window.location.reload(true); }
        else { this.userAvatarInput = null; alert('프로필 이미지 변경 실패'); }
      }
    },
    userAvatarDialogClose(){
      this.userAvatarInput = null
      this.userAvatarDialog = false
    },

    async userPwdSubmit(userPwdInput){
      if(!userPwdInput) alert('비밀번호를 입력해 주세요')
      else{
        let { status } = await this.$axios.put('/api/user/change-password', { userIndex: this.userInfo.userIndex, userPwdInput: userPwdInput } )
        if(status === 200) { this.userPwdInput = null; this.userPwdDialog = false; window.location.reload(true); }
        else { this.userPwdInput = null; alert('비밀번호 변경 실패'); }
      }
    },
    userPwdDialogClose(){
      this.userPwdInput = null
      this.userPwdDialog = false
    },

    async userMoneySubmit(userMoneyInput){
      if(!userMoneyInput) alert('충전할 돈을 입력해 주세요')
      else{
        let { status } = await this.$axios.put('/api/user/add-money', { userIndex: this.userInfo.userIndex, userMoneyInput: userMoneyInput} )
        if(status === 200) { this.userMoneyInput = null; this.userMoneyDialog = false; window.location.reload(true); }
        else { this.userMoneyInput = null; alert('돈 충전 실패'); }
      }
    },
    userMoneyDialogClose(){
      this.userMoneyInput = null
      this.userMoneyDialog = false
    },

    commaUserMoney(money){
      money += ''
      money.replace(/,/gi, '')
      return Number(money).toLocaleString()
    },

    ...mapMutations([
      'isLogin_to_False'
    ]),
    async deleteAccountSubmit(){
      let { status } = await this.$axios.delete('/api/user/delete-account')
      if(status === 200){
        this.deleteAccountDialog = false
        this.isLogin_to_False()
        window.location.reload(true)
      }
      else { alert('회원 탈퇴 실패 했습니다'); }
    },
    deleteAccountDialogClose(){
      this.deleteAccountDialog = false
    },

    async changeToAdmin(){
      let { status } = await this.$axios.put('/api/user/change-admin')
      if(status === 200) { alert('관리자 계정으로 전환되었습니다'); window.location.reload(true); }
      else { alert('이미 관리자 계정입니다') }
    }
  }
}
</script>
