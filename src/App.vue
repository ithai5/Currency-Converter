<template >
  <div id="app">

      <Header class="header"></Header>
      <BodyContent></BodyContent>
      <Footer></Footer>

  </div>
</template>

<script>
import Header from "@/components/layout/Header";
import Footer from "@/components/layout/Footer";
import BodyContent from "@/components/layout/BodyContent";
export default {
  name: 'App',
  components: {
    BodyContent,
    Header,
    Footer,
  },
}
</script>


<!-- global css -->
<style>
.container{

}
#app {
  text-align: center;
  height: 900px;
  top: 0px;
  background-color: #e9e9e9;
}
.inputBox{
  height: 38px;
  top: 198px;

  background: #FFFFFF;
  border: 1px solid #D0D0D0;
  box-sizing: border-box;
  border-radius: 32px;

}

.header{
  position: relative;
  height: 159px;
  top: 0px;

  background: linear-gradient(180deg, #A7CDE9 0%, #8CB3CF 85.89%);
  box-shadow: 0px 4px 28px rgba(0, 0, 0, 0.08);
  border-radius: 8px;
}




</style>
