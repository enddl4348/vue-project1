<template>
   <div class="black-bg" v-if="모달창열렸니 ==  true"> <!-- 데이터 "모달창열렸니가 true 일때만 HTML 보여줘" -->
    <div class="white-bg">
      <img :src="원룸들[누른거].image" class="pop-img">
      <h4>{{ 원룸들[누른거].title }}</h4>
      <p>{{ 원룸들[누른거].content }}</p>
      <!-- 유저가 <input>에 입력한 값을 데이터로 저장하는법1 : @input="" -->
      <!-- <input type="text" @input="month = $event.target.value"> @input - 입력할 때 마다 뭐 실행해주셈, $event.target - js의 e.preventDefault() 의 e랑 같음 -->
      <!-- 유저가 <input>에 입력한 값을 데이터로 저장하는법2 : v-model -->
      <input v-model.number="month"> <!-- v-model.number : 들어오는 값을 숫자로 인식하게 함 -->
      <p>{{ month }}개월 선택함 : {{ 원룸들[누른거].price * month }} 원</p>
      <button @click="$emit('closeModal')">모달창 닫기</button> 
      <!-- 주의) props는 read-only임 받아온거 수정안됨 -->
    </div>  
  </div>
</template>

<script>
export default {
  name : 'ModalPage' ,
  data(){
    return {
      month : 1,
    }
  },
  watch : { //데이터 감시하려면 watch : { 감시할데이터(){} }
    month(a){ //month라는 데이터가 변할 때마다 여기있는 코드 실행됨,여기서 a(파라미터)는 month 
      // 사용자가 month에 입력하 데이터가 13보다 크면 경고문 띄우기
      if(a >= 13){
        alert('13이상 입력하지 마세요');
      }
      //사용자가 글자를 입력하면 alert() 띄우기 + month값을 1로 되돌리기
      if( isNaN(a) == true ){ //숫자가 아닌게 맞으면
        alert('글자는 입력하지 마세요');
        this.month = 1;
      }
    },
  },
  props : {
  /* 
    자식 컴포넌트가 부모가 갖고 있는 데이터 쓰려면 props로 데이터를 전송해야 함
    step1. 밑에 데이터 골라서 보내셈 <자식 :데이터="데이터" />  
    step2. 자식은 props로                                                                                    받은거 등록 -> props: {데이터 이름 : 자료형 이름} 
    Modal.vue가 App.vue에 있는 데이터 수정하고 싶으면 custom event를 씁시다.
  */
    원룸들 : Array,
    누른거 : Number,
    모달창열렸니 : Boolean,
  },

  beforeUpdated(){ //lifecycle hook - beforeUpdated() : 뭔가가 update되면 
    // input에 값이 2가 들어오면 alert창 띄우기
    if (this.month == 2){
      alert('2개월은 너무 적음.. 3개월부터 가능합니다.');
    } 
  },

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