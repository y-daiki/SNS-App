<template>
 <div class="users">
   <user v-for="(user, index) in users" :key="index" :user="user"/>
 </div>
</template>

<script>
import User from '~/components/User.vue'
import { db, firebase } from '~/plugins/firebase'
export default {
 components: {
   User
 },
 data () {
    return {
      users: [],
      displayName: null,
      photoURL: null,
      beLiked: false
    }
  },
  mounted () {
    db.collection('users').onSnapshot((snapshot) => {
      snapshot.docChanges().forEach((change) => {
        const doc = change.doc
        if (change.type === 'added') {
          this.users.unshift({ id: doc.id, ...doc.data() })
        }
      })
    })
  }
}
</script>
