<template>
  <div class="mypage wrapper">
    <div v-if="currentUser">
      <div class="user flex justify-between px-8 my-8">
        <div class="flex">
          <div class="user-avatar mr-4">
            <div
              class="mypage-user-image rounded-full bg-cover bg-center mr-2"
              :style=" 'background-image: url(' + currentUser.photoURL + ')' "
            ></div>
          </div>
          <div class="mypage-user-name vertical-middle">
            <p>{{ currentUser.displayName }}</p>
          </div>
        </div>
      </div>
      <div class="tab flex justify-around">
        <div class="post-count text-center">
          Post
          <br>
          <span class="text-xs">{{ countPost }}</span>
        </div>
        <div class="text-center">
          Following
          <br>
          <span class="text-xs">0</span>
        </div>
        <div class="text-center">
          Follower
          <br>
          <span class="text-xs">0</span>
        </div>
      </div>
    </div>
    <div class="title">
      <h1 class="title-text">クチコミ</h1>
    </div>
    <div class="grid overflow-scroll mb-24 mypage-post-box">
      <div v-for="(post, index) in posts" :key="index" :post="post">
        <div v-if="post.userId === currentUser.uid">
          <div class="post-image w-full">
            <img :src="post.image" alt="">
          </div>
          <div class="actions my-2 ml-4 flex">
            <img v-if="beLiked" src='/images/heart_active.svg' class="w-6 mr-3">
            <img v-else src='/images/heart.svg' class="w-6 mr-3">
            <p>0</p>
          </div>
          <div class="message mx-4 text-sm">
            <p>{{ post.text }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { db, auth, firebase } from '~/plugins/firebase'
  export default {
    data() {
      return {
        posts: [],
        currentUser: {},
        beLiked: false,
        countPost: 0
      };
    },
    created() {
      auth.onAuthStateChanged(user => {
        this.currentUser = user
      })
    },
    mounted () {
      db.collection('posts').onSnapshot((snapshot) => {
        snapshot.docChanges().forEach((change) => {
          const doc = change.doc
          if (change.type === 'added') {
            this.posts.unshift({ id: doc.id, ...doc.data() })
          }
        })
        this.posts.forEach((post) => {
          if (post.userId === this.currentUser.uid) {
            this.countPost = this.countPost + 1
          }
        })
      })
    },
    // methods: {
    //   update() {
    //     db.collection('posts').doc(this.post.id).set({
    //       title: this.post.title,
    //       tags: this.tags,
    //       image: this.post.image,
    //       content: this.post.content,
    //       createdAt: date
    //     }, {merge: true} )
    //   }
    // }
  }
</script>


