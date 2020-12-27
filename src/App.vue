<template >
  <div id="app">
    <div class="container" >
      <Header class="header"></Header>
      <BodyContent></BodyContent>
      <Footer></Footer>
    </div>
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
  text-align: center;
}
#app {

  height: 900px;
  left: 351px;
  top: 0px;
  background-color: #e9e9e9;
}


.header{
  position: relative;
  width: 738px;
  height: 159px;
  left: 351px;
  top: 0px;

  background: linear-gradient(180deg, #A7CDE9 0%, #8CB3CF 85.89%);
  box-shadow: 0px 4px 28px rgba(0, 0, 0, 0.08);
  border-radius: 8px;
}




</style>
