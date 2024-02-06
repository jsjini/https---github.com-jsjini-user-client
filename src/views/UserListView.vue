<template>
  <div class="container">
    <h1>전체 회원 조회</h1>
    <table class="table">
      <caption>Total : {{ count }}</caption>
      <thead>
        <tr>
          <th>No.</th>
          <th>ID</th>
          <th>이름</th>
          <th>성별</th>
          <th>연령</th>
          <th>가입날짜</th>
        </tr>
      </thead>
      <tbody>
        <!--for 과 if를 같이 사용은 불가능하다고 생각해라-->
        <tr v-for="(user, idx) in userList" :key="idx"
          @click="goToUserInfo(user.user_id)" >
          <td>{{ user.user_no }}</td>
          <td>{{ user.user_id }}</td>
          <td>{{ user.user_name }}</td>
          <td v-text="user.user_gender"></td> <!--이렇게 사용도 가능-->
          <td>{{ user.user_age }}</td>
          <td>{{ user.join_date }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template> <!--template태그는 표준태그면서 랜더링은 되지 않는다.-->
<script>
import axios from 'axios'

export default {
  data () {
    return {
      userList : []
    }
  },
  computed: { // 데이터 옵션을 기반으로 해서 새로운 '값'을 생성.
    count() {
      return this.userList.length;
    }
  },
  // watch: {  // 이미 존재하는 값들을 감시. 값이 아니고 '프로세스'라 리턴이 없음.
  //   // 감시한다는 점은 computed와 동일하다.  watch옵션을 벗어나면 사용불가.
  //   userList(newQuestion, oldQuestion) {
  //     console.log('이전 : ', oldQuestion);
  //     alert('데이터가 변경되었습니다.');
  //     console.log('이후 : ', newQuestion);
  //   }
  // },
  created() {
    this.getUserList(); // 비동기작업
  },
  methods : {
    async getUserList() { // 동기작업
      let result = await axios.get('/api/users')
                              .catch(err => console.log(err));
      let list = result.data;
      this.userList = list;
    },
    goToUserInfo(userId) { 
      this.$router.push({ path: '/userInfo', query: { "userId" : userId } }); // $router는 무조건 get방식
      // path 말고 name 사용도 가능.
      // this.$router.push({ name: 'userInfo', query: { "userId" : userId } }); // $router는 무조건 get방식
    }
  }
}
</script>
