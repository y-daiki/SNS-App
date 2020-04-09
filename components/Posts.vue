<template>
  <div>
    <div v-if="!currentUser">
      <div class="home-cover hidden sm:block">
        <div class="home-cover-text">Mens Cosme<br>クチコミサイト</div>
      </div>
      <div class="home-cover-sp sm:hidden">
        <div class="home-cover-text-sp">Mens Cosme<br>クチコミサイト</div>
        <div class="py-6 flex justify-center">
          <el-button
            class="sp-home-button vertical-middle cursor-pointer"
            @click="signIn"
          >ログイン</el-button>
        </div>
      </div>
      <div class="home-title wrapper">
        <div class="home-title-text">最新のクチコミ</div>
      </div>
    </div>
    <div class="posts overflow-scroll mb-24 grid wrapper">
      <post v-for="(post, index) in posts" :key="index" :post="post" :currentUser="currentUser" />
    </div>
  </div>
</template>

<script>
import Post from '~/components/Post.vue'
import { auth, db, firebase } from '~/plugins/firebase'

export default {
  components: {
    Post
  },
  data () {
    return {
      posts: [],
      imageUrl: null,
      text: null,
      currentUser: {}
    }
  },
  created() {
    auth.onAuthStateChanged(user => {
      this.currentUser = user
    })
  },
  methods: {
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
    }
  },
  mounted () {
    db.collection('posts').onSnapshot((snapshot) => {
      snapshot.docChanges().forEach((change) => {
        const doc = change.doc
        if (change.type === 'added') {
          this.posts.unshift({ id: doc.id, ...doc.data() })
        }
      })
    })
  }
}
</script>
