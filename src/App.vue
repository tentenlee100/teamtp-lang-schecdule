<template>
  <div >
    <b-navbar type="dark" variant="info">
      <b-navbar-brand href="#">Team TP 浪行程表-轉換圖片</b-navbar-brand>
    </b-navbar>
    <div class="container-fluid">
      <div class="my-font">
        <p>adgasfgwegefgdsfgsdf Team TP 浪行程表-轉換圖片</p>
      </div>
      <SchecduleImage :schecduleArray="datas" date="7/1"></SchecduleImage>
    </div>

  </div>
</template>

<script>
import SchecduleImage from '@/components/SchecduleImage'

export default {
  name: 'app',
  data: () => ({
    datas: [
      {
        time: '10:00',
        member: ["林于馨"]
      },
      {
        time: '18:00',
        member: ["李采潔"]
      },
      {
        time: '19:30',
        member: ["王逸嘉"]
      },
      {
        time: '20:00',
        member: ["小山美玲","蔡伊柔"]
      },
      {
        time: '21:00',
        member: ["高云珏","劉潔明","邱品涵","潘姿怡"]
      },
    ]
  }),
  components: {
    SchecduleImage
  },
}
</script>

<style>
/* @import url('https://fonts.googleapis.com/css?family=Noto+Sans+TC:400,700&display=swap&subset=chinese-traditional'); */
/* @import url('https://db.onlinewebfonts.com/c/f4b819bb8e3dcf620c21806e8036adbf?family=Langdon'); */
/* .my-font{
font-family: 'Noto Sans TC', sans-serif;
font-weight: 700;
font-size: 80px;
} */
</style>
