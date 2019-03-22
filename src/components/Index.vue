<template>
	<!-- 根容器 -->
	<div class="container">
		<router-link to="/new_course"><button class="btn2">新建班课</button></router-link>
		<div class="top">
		<p>进行中的班课</p>
		<p>{{courses.length}}个进行中的班课</p>
		</div>
		<hr />
		<div class="course-container">
			<div class="course" v-for="(course, index) in courses" :key="index">
				<div class="course-cover">
					<router-link :to="'/course/' + course.courseId">
						<img :src="course.cover" />
					</router-link>
				</div>
				<div class="course-class">
					<p class="title">{{ course.courseClass }}</p>
				</div>
				<div class="cinformation">
				<div class="course-name">
					<p class="title">{{ course.courseName }}</p>
				</div>
				<div class="course-code" v-if="loginUserId === course.userId">
					{{ course.courseCode }}
					
					
				</div>
				</div>
			</div>
		</div>
		<div class="top">
		<p>已结束的班课</p>
		<p>{{courses1.length}}个已结束的班课</p>
		</div>
		<hr />
		<div class="course-container2">
			<div class="course" v-for="(course, index) in courses1" :key="index">
				<div class="course-cover">
					<router-link :to="'/course/' + course.courseId">
						<img :src="course.cover" />
					</router-link>
				</div>
				<div class="course-class">
					<p class="title">{{ course.courseClass }}</p>
				</div>
				<div class="cinformation">
				<div class="course-name">
					<p class="title">{{ course.courseName }}</p>
				</div>
				<div class="course-code2" v-if="loginUserId === course.userId">
					{{ course.courseCode }}
					<button @click="deleteCourse(course.courseId,index)" class="btn1">删除</button>
				</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Index',
	data() {
		return {
			loginUserId: 1,
			courses: [],
			courses1: []
		};
	},
	methods: {
		deleteCourse: function(courseId,index) {
			var _this = this;
			this.$http({
				method: 'delete',
				url: 'http://localhost:8082/api/course/' + courseId
			}).then(function() {
				alert('班课删除成功');
				 _this.courses.splice(index,1);
			});
		}
	},
	created() {
		var _this = this;
		this.$http.get('http://localhost:8082/api/courses2').then(function(response) {
			_this.courses = response.data;
		});
		this.$http.get('http://localhost:8082/api/courses1').then(function(response) {
			_this.courses1 = response.data;
		});
	},
};
</script>
<style scoped>
	
.container {
	padding-top: 20px;
}
.top{
	width: 80%;
	margin: 0 auto;
	display: flex;
	justify-content: space-between;
}


.course-container {
	display: flex;
	flex-wrap: wrap;
}
.course-container2 {
	display: flex;
	flex-wrap: wrap;
	-webkit-filter: grayscale(100%);
	-moz-filter: grayscale(100%);
	-ms-filter: grayscale(100%);
	-o-filter: grayscale(100%);
	filter: grayscale(100%);
				
	filter: gray;
}
.course {
	width: 250px;
	height: 400px;
	margin-right: 20px;
	margin-bottom: 30px;
	background-color: #fff;
	display: flex;
	flex-direction: column;
	padding-bottom: 10px;
}
.course-cover img {
	width: 100%;
	height: 280px;
}
.title {
	font-size: 16px;
	color: #333;
}
.btn1 {
	width: 60px;
	height: 30px;
	border: 1px solid #fff;
	background-color: rgb(0, 187, 221);
	border-radius: 20px;
	outline: none;
	color: #fff;
	font-size: 16px;
}
.btn2 {
	width: 100px;
	height: 50px;
	border: 1px solid #fff;
	background-color: rgb(0, 187, 221);
	border-radius: 20px;
	outline: none;
	color: #fff;
	font-size: 16px;
}
.cinformation{
	display: flex;
}
.course-name{
	flex: 1;
	
}
.course-code {
	color: rgb(0, 187, 221);
	flex: 1;
	padding-top: 18px;
	
}
.course-code2 {
	color: rgb(0, 187, 221);
	flex: 1;
	padding-top: 13px;
	
}
</style>