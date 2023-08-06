<template>

  <div>
    <img :src="원룸.image" class="room-img">
    <h4 @click="$emit('openModal', 원룸.id)">{{ 원룸.title }}</h4> <!-- 누른거 = i  :클릭한 상품의 변호를 파악 -->
    <!-- 
      카드 안의 제목을 눌러야 모달창 뜨게
      custom event
      부모에게 메세지 보낼땐 $emit('작명', 데이터)
      제목을 누르면  원룸.id 담아서 메세지 발사해주세요~
      원룸.id는 몇번째 원룸 클릭했는지 순서를 가리킴
    -->
    <p>{{ 원룸.price }}원</p>
  </div>

</template>

<script>
export default {
  name : 'CardList' ,
  props : {
  /* 
    자식 컴포넌트가 부모가 갖고 있는 데이터 쓰려면 props로 데이터를 전송해야 함
    step1. 밑에 데이터 골라서 보내셈 <자식 :데이터="데이터" />  
    step2. 자식은 props로 받은거 등록 -> props: {데이터 이름 : 자료형 이름} 
    props로 보낸 데이터는 Read-only
    */
    원룸 : Object,
  }
}
</script>

<style>
</style>