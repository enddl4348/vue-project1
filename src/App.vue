<template>
	<!-- v-if 더 알아야 할 거 -->
	<!-- v-else 
  위에 있는 v-if가 참이 아니면 이걸 보여주세요~ -->
	<div v-if="1 == 2">안녕하세요</div>
	<div v-else>안녕하세요2</div>

	<!-- 
    (모달창 component화 시킨거 - props 포함)
    자식 컴포넌트가 부모가 갖고 있는 데이터 쓰려면 props로 데이터를 전송해야 함
    step1. 밑에 데이터 골라서 보내셈 <자식 :데이터="데이터" />  
    step2. 자식은 props로 받은거 등록 -> props: {데이터 이름 : 자료형 이름}
  -->

	<!-- css로 에니메이션 주려면 1.시작전 class명 2.에니메이션 끝난 후 class명 -->

	<!-- <div class="start" :class="{ end : 모달창열렸니 }">  :class="{ 클래스명 : 조건부 }" - class명 'end'는 뒤에 있는 조건부가 true일때만 붙음 
    <ModalPage @closeModal="모달창열렸니 = false" :원룸들="원룸들" :누른거="누른거" :모달창열렸니="모달창열렸니" />
  </div> -->

	<!-- css로 에니메이션 주려면 2.에니메이션 주고싶은 요소를 <transition name="작명">으로 감싸기 -->

	<transition name="fade">
		<ModalPage
			@closeModal="모달창열렸니 = false"
			:원룸들="원룸들"
			:누른거="누른거"
			:모달창열렸니="모달창열렸니"
		/>
	</transition>

	<!-- 데이터 "모달창열렸니가 true 일때만 HTML 보여줘" -->
	<!-- <div class="black-bg" v-if="모달창열렸니 ==  true"> 
    <div class="white-bg">
      <img :src="원룸들[누른거].image" class="pop-img">
      <h4>{{ 원룸들[누른거].title }}</h4>
      <p>{{ 원룸들[누른거].content }}</p>
      <p>{{ 원룸들[누른거].price }} 원</p>
      <button @click="모달창열렸니 = false">모달창 닫기</button>
    </div>  
  </div> -->

	<div class="menu">
		<a v-for="menu in 메뉴들" :key="menu"> {{ menu }} </a>
	</div>

	<!-- 
    (HTML component화)

    *컴포넌트 문법 왜 쓰는가?
    -재사용 쉬움
    -알아보기 쉬움

    *축약해둔 컴포넌트 쓰는 법
    1. vue파일 import해오고 2.등록하고 3.쓰셈
  -->

	<!-- 컴포넌트의 lifecycle -->
	<DiscountBanner v-if="showDiscount == true" />

	<button @click="priceSort">가격순 정렬</button>
	<button @click="sortBack">되돌리기</button>

	<!-- 반복문 이용해서 각각 원룸에 1씩 증가시키는 버튼 추가 예제 -->
	<div v-for="(a, i) in 신고수" :key="i">
		<h4 :style="스타일">{{ products[0] }}cc 원룸</h4>
		<p>{{ price1 }} 만원</p>
		<button @click="increase(i)">허위매물신고(클릭시 1증가)</button>
		<!-- 함수 방식 -->
		<button @mouseover="신고수[i] += 2">
			허위매물신고(마우스오버시 2증가)
		</button>
		<!-- 동작 직접 기입 방식 -->
		<span>신고수: {{ 신고수[i] }}</span>
	</div>

	<br /><br /><br /><br /><br />

	<div>
		<img src="./assets/room0.jpg" class="room-img" />
		<h4 @click="모달창열렸니 = true">{{ products[0] }} 원룸</h4>
		<p>{{ price2 }} 만원</p>
		<button @click="신고수[0] += 1">허위매물신고</button>
		<span> 신고수 : {{ 신고수[0] }}</span>
	</div>

	<br /><br /><br /><br /><br />

	<!-- 실제 데이터 export해서 작업한 예제 -->
	<div v-for="(a, i) in 원룸들" :key="i">
		<img :src="a.image" class="room-img" />
		<h4
			@click="
				모달창열렸니 = true;
				누른거 = i;
			"
		>
			{{ a.title }}
		</h4>
		<!-- 누른거 = i  :클릭한 상품의 변호를 파악 -->
		<p>{{ a.price }}원</p>
	</div>

	<br /><br /><br /><br /><br />

	<!-- 위 내용을 card라는 이름으로 컴포넌트화 시킨 거 -->
	<!-- <CardList :원룸="원룸들[0]" />
  <CardList :원룸="원룸들[1]" />
  <CardList :원룸="원룸들[2]" />
  <CardList :원룸="원룸들[3]" />
  <CardList :원룸="원룸들[4]" />
  <CardList :원룸="원룸들[5]" /> -->

	<!-- 위에를 반복문으로 줄인거 -->
	<!-- 
    부모가 자식이 보낸 메세지 수신할 땐 @작명한거="" 
    자식이 openModal 이름의 메세지 보내면 원하는 기능 자바스크립트 실행해주셈~
    자식이 보낸 데이터는 $event 변수에 담겨있음
  -->
	<CardList
		@openModal="
			모달창열렸니 = true;
			누른거 = $event;
		"
		:원룸="원룸들[i]"
		v-for="(작명, i) in 원룸들"
		:key="작명"
	/>

	<!--  
    {{데이터바인딩}}
    -JS 데이터를 HTML에 꽂아넣는 문법
    -문법: {{데이터이름}}
    {{데이터바인딩}}하는 이유
    1.HTML에 하드코딩 해놓으면 나중에 변경 어려움
    2.Vue의 실시간 자동 렌더링 쓰려면 하셈
    3.HTML 속성도 데이터바인딩 가능 -문법  :속성="데이터이름"
    4.정리 : 자주 변경될 데이터들은 밑에 저장해놨다가 데이터 바인딩
  -->

	<!-- 
    Vue의 HTML 반복문
    1.문법 : <태그 v-for="작명 in 몇회"> ( :key="작명" 도 필요)
    * :key=""의 용도 *
      -반복문 쓸 때 꼭 써야함
      -반복문 돌린 요소를 컴퓨터가 구분하기 위해 씀
    2.array/object 집어넣기 가능 - 그럼 자료안의 데이터 갯수만큼 반복됨,작명한 변수는 데이터안의 자료가 됨
    3.변수 작명 2개까지 가능 - 왼쪽 변수는 array의 내의 데이터, 오른쪽 변수는 1씩 증가하는 정수 
  -->

	<!-- 
    Vue 이벤트 헨들러
    1.문법 : <태그 @click="자바스크립트 동작~">
    2.여러가지 이벤트 사용가능( click, mouseover)
  -->

	<!-- 
    동적인 UI 만드는 법
    1.UI의 현재 상태를 데이터로 저장해둠 - > "모달창이 지금 어떻게 보여야 함"
    2.데이터에 따라 UI가 어떻게 보일지 작성 
  -->

	<!-- 
    v-if="조건식" 
    조건식이 참일때만 해당 HTML 보여줌
  -->

	<!-- 
    데이터는 한 곳에 보관함
    그리고 필요하면 가져다 씀
  -->
</template>

<script>
import data from "./assets/oneroom.js";
import DiscountBanner from "./Discount.vue";
import ModalPage from "./Modal.vue";
import CardList from "./Card.vue";

export default {
	name: "App",
	data() {
		/* 데이터 보관함 */
		return {
			/* 여기에 데이터 있어야 함 */
			price1: 60 /* 데이터는 object 자료료 저장 {자료이름 : 자료내용} */,
			price2: 70,
			스타일: "color : red",
			메뉴들: ["Home", "Shop", "About"],
			products: ["역삼동원룸", "천호동원룸", "마포구원룸"],
			신고수: [0, 0, 0],
			모달창열렸니: false, //현재 UI 상태
			원룸들오리지널: [...data], //원본 -> array/object 데이터의 각각 별개의 사본을 만들려면 [...array자료]
			원룸들: data, //oneroom.js 에서 export 해온 data
			누른거: 0,
			showDiscount: true,
		};
	},
	methods: {
		/* 
      Vue에서 함수 만들고 싶으면 methods:{}안에 만듬
      *함수 만들 때 주의사항 
        -함수안에서 데이터 쓸 땐 this.데이터명 
    */
		increase(i) {
			this.신고수[i] += 1;
		},
		priceSort() {
			// sort()문법 : 숫자순으로 정렬
			this.원룸들.sort(function (a, b) {
				//sort()는 원본은 보존 안됨
				return a.price - b.price;
			});
		},
		sortBack() {
			this.원룸들 = [...this.원룸들오리지널]; //array/object 데이터의 각각 별개의 사본을 만들려면 [...array자료]
		},
	},

	// App.vue에 mount()사용하면 App.vue(메인페이지)가 mount 되고나서 코드 실행
	mounted() {
		//lifecycle hook 쓰려면
		// 2초 후에 사라지게
		setTimeout(() => {
			//arrow function으로 써야 바깥에 있는 this를 쓸 수 있음
			this.showDiscount = false;
		}, 2000);
	},

	components: {
		// 컴포넌트 등록
		DiscountBanner: DiscountBanner,
		ModalPage: ModalPage,
		CardList: CardList,
	},
};
</script>

<style>
.start {
	opacity: 0;
	transition: opacity 1s;
}
.end {
	opacity: 1;
}
/* vue animation에 있는 css - vue로 에니메이션 넣는 방법2*/
/* 시작 에니메이션 */
.fade-enter-from {
	/* 시작 스타일 */
	transform: translateY(-1000px);
}
.fade-enter-active {
	/* transition */
	transition: all 1s;
}
.fade-enter-to {
	/* 끝날 때 스타일 */
	transform: translateY(0);
}
/* 퇴장 에니메이션 */
.fade-leave-from {
	opacity: 1;
}
.fade-leave-active {
	transition: all 1s;
}
.fade-leave-to {
	opacity: 0;
}
body {
	margin: 0;
}
div {
	box-sizing: border-box;
}
.discount {
	background: #eee;
	padding: 10px;
	margin: 10px;
	border-radius: 5px;
}
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
}

.menu {
	background: darkslateblue;
	padding: 15px;
	border-radius: 5px;
}
.menu a {
	color: white;
	padding: 10px;
}
.room-img {
	width: 100%;
	margin-top: 40px;
}
.black-bg {
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, 0.5);
	position: fixed;
	padding: 20px;
}
.white-bg {
	width: 100%;
	background-color: white;
	border-radius: 8px;
	padding: 20px;
}
.pop-img {
	width: 80%;
}
</style>
