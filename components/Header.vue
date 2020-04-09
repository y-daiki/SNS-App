<template>
<header class="l-header">
  <div class="l-header-box flex justify-center sm:justify-between wrapper">
    <nuxt-link to="/" class="cursor-pointer vertical-middle">
      <img src="/images/logo.png" class="c-header-logo py-2 ">
    </nuxt-link>
    <div v-if="!currentUser" class="py-6 hidden sm:block">
      <div
        class="text-sm cursor-pointer bg-blue-500 hover:bg-blue-700 text-white font-bold py-1 px-4 rounded"
        @click="signIn"
      >Googleアカウントでログイン</div>
    </div>
    <div v-if="currentUser" @click="openModal" class="sp-post-icon py-5 cursor-pointer sm:hidden">
        <i class="el-icon-edit"></i>
    </div>
    <div v-if="currentUser" class="header-nav sm:hidden">
      <div id="nav-drawer">
        <input id="nav-input" type="checkbox" class="nav-unshown">
        <label id="nav-open" for="nav-input"><span></span></label>
        <label class="nav-unshown" id="nav-close" for="nav-input"></label>
        <div id="nav-content">
          <nuxt-link to="/users/tekitou">
            <div  class="header-currentUser-sp p-6 flex">
              <div class="user-avatar mr-1 vertical-middle">
                <div
                  class="w-10 h-10 rounded-full bg-cover bg-center mr-2"
                  :style=" 'background-image: url(' + currentUser.photoURL + ')' "
                ></div>
              </div>
              <div class="header-user-name vertical-middle">
                <p>{{ currentUser.displayName }}</p>
              </div>
            </div>
          </nuxt-link>
          <nuxt-link to="/users">
            <div class="sp-menu-box text-sm cursor-pointer">ユーザーを探す</div>
          </nuxt-link>
          <div class="sp-menu-box">
            <div @click="signOut" class="text-sm cursor-pointer">ログアウト</div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="currentUser" class="sm:flex justify-between hidden">
      <ul class="menu vertical-middle">
        <li class="menu__single">
            <div class="init-bottom">
              <div class="header-currentUser hidden sm:flex">
                <div class="user-avatar mr-1 vertical-middle">
                  <div
                    class="w-10 h-10 rounded-full bg-cover bg-center mr-2"
                    :style=" 'background-image: url(' + currentUser.photoURL + ')' "
                  ></div>
                </div>
                <div class="header-user-name vertical-middle">
                  <p>{{ currentUser.displayName }}</p>
                </div>
              </div>
            </div>
            <ul class="menu__second-level">
                <li class="menu-item"><nuxt-link to="/users/tekitou" class="cursor-pointer">マイページ</nuxt-link></li>
                <li class="menu-item"><nuxt-link to="/users" class="cursor-pointer">ユーザーを探す</nuxt-link></li>
                <li class="menu-item"><div @click="signOut" class="cursor-pointer">ログアウト</div></li>
            </ul>
        </li>
      </ul>
      <div class="post-button-box flex justify-center">
        <div @click="openModal" class="w-30 post py-5 cursor-pointer">
          <div class="button">
            <i class="el-icon-edit"></i>
            <p class="header-post-text">投稿</p>
          </div>
        </div>
      </div>
    </div>
  </div>
   <div v-if="modalVisible" class="modal">
      <div class="wrapper">
        <div class="title flex justify-center sm:block">
          <div class="back-btn-sp cursor-pointer sm:hidden" @click="modalVisible = false">
            <img src="/images/back.svg" class="h-4">
          </div>
          <h1 class="title-text">クチコミの投稿</h1>
        </div>
        <div class="modal_content">
          <h2 class="modal-subheading">画像を追加</h2>
          <!-- <el-upload
            class="avatar-uploader"
            action=""
            :show-file-list="false"
            :http-request="uploadFile">
            <img v-if="imageUrl" :src="imageUrl" class="upload-avatar">
            <div v-else class="avatar-uploader-icon">
              <div class="el-icon-plus"></div>
            </div>
          </el-upload> -->
          <div class="input-item">
            <label v-if="!imageUrl"  class="input-item__label">
              <div class="avatar-uploader-icon">
                <div class="el-icon-plus"></div>
              </div>
              <input type="file" class="hidden" @change="onFileChange" />
            </label>
            <div class="preview-item">
              <img
                v-show="imageUrl"
                class="upload-avatar"
                :src="imageUrl"
                alt=""
              />
            </div>
          </div>
          <h2 class="modal-subheading">レビューを追加</h2>
          <form>
            <textarea
              name="テキストエリア"
              rows="8"
              maxlength="3000"
              class="textarea xmt-8"
              placeholder="自分用のメモ代わりでもOK！使ってみた感想を気軽に書いてみましょう！商品の画像は必須じゃないけど、あると参考になります☆"
              v-model="text"
            >
            </textarea>
          </form>
          <div v-if="!text" class="post-btn opacity-50 cursor-not-allowed">
            投稿する
          </div>
          <div v-else class="post-btn cursor-pointer" @click="post">
            投稿する
          </div>
          <div class="actions mt-4 px-8 ">
            <div class="back-btn sm:flex justify-between cursor-pointer hidden" @click="modalVisible = false">
              <img src="/images/back.svg" class="h-4">
              <p class="back-text">内容を削除してホームに戻る</p>
            </div>
          </div>
        </div>
      </div>
    </div>
</header>
</template>

<script>
  import { db, auth, firebase } from '~/plugins/firebase'
  export default {
    data() {
      return {
        activeIndex: '1',
        currentUser: {},
        modalVisible: false,
        imageUrl: null,
        text: null,
      };
    },
    created() {
      auth.onAuthStateChanged(user => {
        this.currentUser = user
      })
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
      signIn () {
        const provider = new firebase.auth.GoogleAuthProvider()
        auth.signInWithPopup(provider)
        .then((result) => {
          alert('Hello, '+result.user.displayName+'!')
          db.collection('users').doc(result.user.uid).set({
            displayName: result.user.displayName,
            photoURL: result.user.photoURL,
            email: result.user.email
          })
        })
      },
      signOut () {
        if (window.confirm('ログアウトしますか?')) {
          auth.signOut()
          .then(() => {
            alert('ログアウトしました')
            this.$router.push({ path: '/' })
          })
        }
      },
      async post () {
        await db.collection('posts').add({
          userName: this.currentUser.displayName,
          userPhoto: this.currentUser.photoURL,
          userId: this.currentUser.uid,
          text: this.text,
          image: this.imageUrl,
          createdAt: new Date().getTime()
        })
        this.modalVisible = false
        this.$router.push({ path: '/' })
      },
      openModal () {
        this.modalVisible = true
        this.text = null
        this.imageUrl = null
      },
      async uploadFile (data) {
        const storageRef = firebase.storage().ref()
        const time = new Date().getTime()
        const ref = storageRef.child(`posts/${time}_${data.file.name}`)
        const snapshot = await ref.put(data.file)
        const url = await snapshot.ref.getDownloadURL()
        this.imageUrl = url
      },
      onFileChange(e) {
        const files = e.target.files || e.dataTransfer.files;
        this.createImage(files[0]);
      },
      createImage(file) {
        const reader = new FileReader();
        reader.onload = e => {
          this.imageUrl = e.target.result;
        };
        reader.readAsDataURL(file);
      }
    }
  }
</script>

<style>

.input-item {
  border: dotted 1px #888;
  width: 180px;
  height: 180px;
}

.menu > li.menu__single {
    position: relative;
}

.menu-item {
  padding: 0 8px;
  font-size: 12px;
  font-weight: 300;
  line-height: 32px;
  letter-spacing: 1px;
  height: 32px;
  border-bottom: 1px solid #e7e7e7;
}

li.menu__single ul.menu__second-level {
    position: absolute;
    top: 40px;
    width: 200px;
    background: #fff;
    -webkit-transition: all .2s ease;
    transition: all .2s ease;
    color: #2D2926;
    visibility: hidden;
    border: 1px solid #e7e7e7;
}

li.menu__single:hover ul.menu__second-level {
    top: 50px;
    visibility: visible;
    opacity: 1;
}

.header-nav {
  position: absolute;
  top: 20px;
  left: 20px;
}

#nav-drawer {
  position: relative;
}

.nav-unshown {
  display:none;
}

#nav-open {
  display: inline-block;
  width: 30px;
  height: 22px;
  vertical-align: middle;
}

#nav-open span, #nav-open span:before, #nav-open span:after {
  position: absolute;
  height: 3px;
  width: 25px;
  border-radius: 3px;
  background: #555;
  display: block;
  content: '';
  cursor: pointer;
}
#nav-open span:before {
  bottom: -8px;
}
#nav-open span:after {
  bottom: -16px;
}

#nav-close {
  display: none;
  position: fixed;
  z-index: 99;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: black;
  opacity: 0;
  transition: .3s ease-in-out;
}

#nav-content {
  overflow: auto;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;
  width: 90%;
  max-width: 300px;
  height: 100%;
  background: #fff;
  transition: .3s ease-in-out;
  -webkit-transform: translateX(-105%);
  transform: translateX(-105%);
}

#nav-input:checked ~ #nav-close {
  display: block;
  opacity: .5;
}

#nav-input:checked ~ #nav-content {
  -webkit-transform: translateX(0%);
  transform: translateX(0%);
  box-shadow: 6px 0 25px rgba(0,0,0,.15);
}
</style>
