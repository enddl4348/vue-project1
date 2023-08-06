<template>
  <div class="discount">
    <h4>지금 결제하면 {{ amount }}% 할인</h4>
  </div>
</template>

<script>
export default {
  name: 'DiscountBanner' ,
  data(){
    return {
      amount : 30,
    }
  },
  mounted(){
    // 1초마다 할인율이 1%씩 감소
    // setInterval(()=>{
    //   this.amount--;
    // },1000);
  }
}
</script>

<style>

/* 
컴포넌트를 만들고 싶으면 
- vue파일 만드셈
- <template>에 축약할 HTML 넣기 
- 심심하면 name 
*/
</style>