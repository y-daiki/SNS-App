<template>
  <div>
    <app-header />
    <nuxt />
    <app-footer />
  </div>
</template>

<script>
  import AppHeader from '~/components/Header.vue'
  import AppFooter from '~/components/Footer.vue'

  export default {
    components: {
      AppHeader,
      AppFooter
    }
  }
</script>

<style>
.wrapper {
  max-width: 900px;
  margin: auto;
}

html {
  font-family: 'Source Sans Pro', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 16px;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.vertical-middle {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.modal {
 background: white;
 width: 100%;
 height: 100%;
 position: fixed;
 top: 0;
}

.uploaded-image {
 max-height: 120px;
}

.post-btn {
 margin: 50px auto 40px;
 padding: 10px 12px;
 border-radius: 5px;
 background-color: rgb(111, 158, 240);
 font-size: 15px;
 color: #fff;
 font-weight: 500;
 text-align: center;
 width: 300px;
}



.el-button {
 width: 100%;
}

.grid {
  display: grid;
  gap: 26px;
  grid-template-columns: repeat(auto-fit, minmax(240px,1fr));
}

.title {
  border-bottom: 0.5px solid #C7BCBC;
  margin-bottom: 10px;
}

.title-text {
  color: #5F5354;
  font-size: 20px;
  margin: 30px 40px 12px;
  font-weight: 400;
  letter-spacing: 1px;
  line-height: 29px;
}

.actions {
  width: 300px;
}

.back-btn {
  text-align: center;
  margin-top: 20px;
}

.back-text {
  font-size: 16px;
  font-weight: 300;
  color: #5F5354;
}

.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  margin: 0;
}
.avatar-uploader .el-upload:hover {
  border-color: #409EFF;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #C7BCBC;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
  background-color: #F0EEEE;
}
.upload-avatar {
  width: 178px;
  height: 178px;
  display: block;
}

.modal-subheading {
  height: 46px;
  line-height: 46px;
  font-size: 14px;
  font-weight: 500;
  color: #5F5354;
  margin: 0px;
}

.modal_content {
  padding: 0 30px;
}

.c-header-logo {
  width: 150px;
}

.button {
  text-decoration: none;
  color: #fff;
  font-size: 15px;
  line-height: 40px;
  letter-spacing: 1.5px;
  color: #fff;
  font-weight: 500;
  text-align: center;
  background: rgb(111, 158, 240);
  width: 360px;
  height: 40px;
  border-radius: 5px;
  width: 112px;
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: 0 20px;
}

.header-user-name {
    max-width: 136px;
    font-size: 14px;
    color: #5F5354;
}

.mypage-user-image {
  width: 144px;
  height: 144px;
}

.mypage-user-name {
  font-size: 24px;
  line-height: 32px;
  letter-spacing: 0.8px;
  color: #000;
  font-weight: 500;
}

#header-currentUser-sp {
  transition: .3s ease-in-out;
}

.sp-menu-box {
  margin-top: 20px;
  margin-left: 30px;
  color: #5F5354;
}

.l-header {
  position: fixed;
  z-index: 10;
  top: 0;
  width: 100%;
  background: #fff;
  box-shadow: 0 1px 4px 0 rgb(111, 158, 240);
}

.post-button-box {
  margin-left: 30px;
}

.tab {
  color: #5F5354;
}

.posts, .users, .mypage {
  margin-top: 100px;
}

.sp-post-icon {
  position: absolute;
  top: 3px;
  right: 30px;
}

.home-cover {
  width: 100%;
  height: 800px;
  background-image: url("/images/cover.jpg");
  background-size: cover;
  background-position: center top;
  margin-top: 80px;
}

.home-cover-sp {
  width: 100%;
  height: 800px;
  background-image: url("/images/cover-sp.jpeg");
  background-size: cover;
  background-position: center top;
  margin-top: 62px;
}

.home-cover-text {
  left: 25%;
  color: #fff;
  font-size: 6rem;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: 500;
  text-align: center;
  padding-top: 250px;
  text-shadow: 4px 4px 10px rgb(111, 158, 240);
}

.home-cover-text-sp {
  left: 25%;
  color: #fff;
  font-size: 50px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: 500;
  text-align: center;
  padding-top: 250px;
  text-shadow: -4px 4px 10px rgb(111, 158, 240);
}

.home-title {
  padding: 0.1px 0;
  margin-top: 100px;
  margin-bottom: 30px;
}

.home-title-text {
  padding: 24px 0;
  border-bottom: 1px solid #e7e7e7;
  border-top: 1px solid #e7e7e7;
  font-size: 18px;
  line-height: 24px;
  letter-spacing: 0.4px;
  color: #2D2926;
  font-weight: 500;
  text-align: center;
}

.textarea {
  width: 100%;
  height: 144px;
  padding: 16px;
  border: 1px solid #988D8D;
  font-size: 14px;
  border-radius: 3px;
  resize: none;
  font-weight: 300;
}

.back-btn-sp {
  position: absolute;
  top: 35px;
  left: 15px;
}

.sp-home-button {
  width: 100px;
}

.user-name {
  color: #5F5354;
}
</style>
