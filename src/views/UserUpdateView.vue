<template>
    <div class="container">
        <h1>회원 정보 수정</h1>
        <div class="row">
            <table class="table">
                <tr>
                    <th class="text-right table-primary">No.</th>
                    <td class="text-center">
                        <input class="form-control" type="number" v-model="userInfo.user_no" readonly>
                    </td>
                </tr>
                <tr>
                    <th class="text-right table-primary">아이디</th>
                    <td class="text-center">
                        <input class="form-control" type="text" v-model="userInfo.user_id" readonly>
                    </td>
                </tr>
                <tr>
                    <th class="text-right table-primary">비밀번호</th>
                    <td class="text-center">
                        <input class="form-control" type="password" v-model="userInfo.user_pwd">
                    </td>
                </tr>
                <tr>
                    <th class="text-right table-primary">이름</th>
                    <td class="text-center">
                        <input class="form-control" type="text" v-model="userInfo.user_name">
                    </td>
                </tr>
                <tr>
                    <th class="text-right table-primary">성별</th>
                    <td class="text-center">
                        남<input type="radio" value="M" v-model="userInfo.user_gender">
                        여<input type="radio" value="F" v-model="userInfo.user_gender">
                        <!-- <input type="checkbox" true-value="예" false-value="아니요" v-model="chkVal"> -->
                    </td>
                </tr>
                <tr>
                    <th class="text-right table-primary">나이</th>
                    <td class="text-center">
                        <input class="form-control" type="number" v-model="userInfo.user_age" min="0" max="150">
                    </td>
                </tr>
            </table>
        </div>
        <div class="row">
            <button class="btn btn-info" @click="updateInfo()">수정</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            // chkVal : "아니요",
            userInfo: {
                user_no: null,
                user_id: "",
                user_pwd: "",
                user_name: "",
                user_gender: null,
                user_age: null
            }
        }
    },
    created() {
        let searchId = this.$route.query.userId; // 요청은 router 받는건 route
        this.userInfo.user_id = searchId;
    },
    methods: {
        updateInfo() {
            if (!this.validation()) return;

            let data = this.getSendData();
            axios.put('/api/users/' + this.userInfo.user_id, data)
            //                   .then(result => {
                              
            // let info = result.data;
            // this.userInfo = info;
                // 3) 결과처리
            // })
            .catch(err => console.log(err));

        },
        validation() {
            if (this.userInfo.user_pwd == "") {
                alert('비밀번호가 입력되지 않았습니다.');
                return false;
            }
            if (this.userInfo.user_name == "") {
                alert('이름이 입력되지 않았습니다.');
                return false;
            }
            return true;
        },
        getSendData() {
            let obj = this.userInfo;
            let delData = ["user_no", "user_id"];
            let newObj = {};

            let isTargeted = null;
            for(let field in obj){
                isTargeted = false;
                for(let target of delData){
                    if(field == target) {
                        isTargeted = true;
                        break;
                    }
                }
                if(!isTargeted){
                    newObj[field] = obj[field];
                }
            }
            let sendData = {
                "param" : newObj
            }
            return sendData;
        }
    }
}

</script>