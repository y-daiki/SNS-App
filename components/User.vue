<template>
 <div class="user p-4 flex justify-between wrapper">
   <div class="user-info flex">
     <div class="avatar mr-4">
       <a><img :src="user.photoURL" class="w-12 h-12 rounded-full"></a>
     </div>
     <div class="displayName vertical-middle">
       <p>{{ user.displayName }}</p>
     </div>
   </div>
   <div class="follow-btn vertical-middle">
     <el-button>Follow</el-button>
   </div>
 </div>
</template>

<script>
  import { db, firebase } from '~/plugins/firebase'
  export default {
    props: ['user']
  }
</script>
