<template>
  <div>
    카카오 로그인 진행중 ...
    <br />
    추가정보
  </div>
</template>

<script>
import { encryptData } from '@/plugins/crypto';
import axios from 'axios';

export default {
    name: "KakaoRedirect",
    created() {
        const code = new URL(window.location.href).searchParams.get("code");
        console.log('kakao redirect', code);
        // 서버로 code 전달
        this.sendCodeToServer(code);
    },
    methods: {
        async sendCodeToServer(code) {
            // 서버로 로그인 요청 (code 값으로 SNS 인증 처리후 accessToken 반환)
            const response = await axios.post("http://localhost:8080/member/kakao/doLogin", { code });
            const token = response.data.token;
            localStorage.setItem("token", encryptData(token));
            window.location.href = "/";
        }
    }
}
</script>

<style scoped>

</style>