<template>
  <Form @submit="signup" :validation-schema="schema" v-slot="{ values }">
    <div class="memberContainer">
      <div class="header">
        <div class="headerInner">
          <a class="headerLogo">
            <span class="blind">INTERPARK</span>
          </a>
          <div class="headerTitle">정보입력</div>
        </div>
      </div>
      <div class="contents">
        <div class="contentWrapper registContent">
          <div class="uBlock">
            <div class="uInputArea emailType">
              <div class="col">
                <div class="uInput">
                  <label for="inputEmail">이메일</label>
                  <div class="inputBox">
                    <Field
                      type="text"
                      id="inputEmail"
                      name="email"
                      class="inputText"
                      placeholder="이메일을 입력해주세요"
                      v-model="member.email"
                    />
                  </div>
                </div>
              </div>
            </div>
            <ErrorMessage class="ErrorMessage" name="email" />
          </div>
          <div class="uBlock">
            <div class="uInputArea">
              <div class="col">
                <div class="uInput">
                  <label for="inputPw">비밀번호</label>
                  <div class="inputBox">
                    <Field
                      type="password"
                      id="inputPw"
                      name="password"
                      class="inputText"
                      placeholder="8~12자 영문, 숫자, 특수문자"
                      v-model="member.password"
                    />
                    <button
                      type="button"
                      class="btnDel"
                      aria-label="삭제"
                      @click="member.password = ''"
                    ></button>
                  </div>
                </div>
              </div>
            </div>
            <ErrorMessage class="ErrorMessage" name="password" />
          </div>
          <div class="uBlock">
            <div class="uInputArea">
              <div class="col">
                <div class="uInput">
                  <label for="inputPwConfirm">비밀번호 확인</label>
                  <div class="inputBox">
                    <Field
                      type="password"
                      id="inputPwConfirm"
                      name="confirm_password"
                      class="inputText"
                      placeholder="8~12자 영문, 숫자, 특수문자"
                      v-model="member.confirm_password"
                    />
                    <button
                      type="button"
                      class="btnDel"
                      aria-label="삭제"
                    ></button>
                  </div>
                </div>
              </div>
            </div>
            <ErrorMessage class="ErrorMessage" name="confirm_password" />
          </div>
          <div class="uBlock">
            <div class="uInputArea">
              <div class="col">
                <div class="uInput">
                  <label for="inputName">이름</label>
                  <div class="inputBox">
                    <Field
                      type="text"
                      id="inputName"
                      name="name"
                      class="inputText"
                      placeholder="이름을 입력해주세요"
                      v-model="member.name"
                    />
                    <button
                      type="button"
                      class="btnDel"
                      aria-label="삭제"
                      @click="member.name = ''"
                    ></button>
                  </div>
                </div>
              </div>
            </div>
            <ErrorMessage class="ErrorMessage" name="name" />
          </div>
          <div class="uBlock">
            <div class="uInputArea">
              <div class="col">
                <div class="uInput">
                  <label for="inputAddress">주소</label>
                  <div class="inputBox">
                    <Field
                      type="text"
                      id="inputAddress"
                      name="region_1depth_name"
                      class="inputText"
                      placeholder="주소를 입력해주세요"
                      v-model="member.region_1depth_name"
                    />
                  </div>
                </div>
              </div>
            </div>
            <ErrorMessage class="ErrorMessage" name="region_1depth_name" />
          </div>
          <div class="uBlock">
            <div class="uInputArea">
              <div class="col">
                <div class="uInput">
                  <label for="inputCellphone">휴대폰</label>
                  <div class="inputBox">
                    <Field
                      type="text"
                      id="inputCellphone"
                      name="cellphone"
                      class="inputText"
                      placeholder="010-1234-5678"
                      v-model="member.cellphone"
                    />
                    <button
                      type="button"
                      class="btnDel"
                      aria-label="삭제"
                      @click="member.cellphone = ''"
                    ></button>
                  </div>
                </div>
              </div>
            </div>
            <ErrorMessage class="ErrorMessage" name="cellphone" />
          </div>
          <div class="uBlock checkBlock">
            <div class="uCheckbox">
              <label>
                <input type="checkbox" v-model="isSmsEmailOptIn" />
                <span class="text">
                  SMS, 이메일로 상품 및 이벤트 정보를 받겠습니다.(선택)
                </span>
              </label>
            </div>
            <div class="uCheckbox">
              <label>
                <input type="checkbox" v-model="isOver14" />
                <span class="text">14세 이상입니다.</span>
              </label>
            </div>
            <div class="uErrorText" v-if="!isOver14">
              14세 미만 가입시 법정대리인 동의 필수입니다.
            </div>
          </div>
          <div class="ubtnArea">
            <div class="col">
              <button
                type="submit"
                class="uBtn point"
                :disabled="
                  !isOver14 ||
                  !values.email ||
                  !values.password ||
                  !values.name ||
                  !values.region_1depth_name ||
                  !values.cellphone
                "
              >
                가입완료
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="footer">
        <div class="copyright">
          Copyright © InterparkTriple Corp. All rights reserved.
        </div>
      </div>
    </div>
  </Form>
</template>

<script setup>
import { ref } from 'vue';
import { Form, Field, ErrorMessage } from 'vee-validate';
import * as yup from 'yup';
import { useMemberStore } from '@/stores/useMemberStore';
import { useRouter } from 'vue-router';

const router = useRouter();

const memberStore = useMemberStore();

const schema = yup.object().shape({
  email: yup
    .string()
    .email('이메일 양식대로 작성 해주세요.')
    .required('이메일은 필수입니다.'),
  password: yup
    .string()
    .min(8, '비밀번호는 최소 8자를 입력해주세요.')
    .required('비밀번호는 필수입니다.'),
  confirm_password: yup
    .string()
    .oneOf([yup.ref('password'), null], '비밀번호가 일치하지 않습니다.')
    .required('비밀번호 확인은 필수입니다.'),
  name: yup.string().required('이름은 필수입니다.'),
  region_1depth_name: yup.string().required('주소는 필수입니다.'),
  cellphone: yup.string().required('휴대폰 번호는 필수입니다.'),
});

const isOver14 = ref(false);
const isSmsEmailOptIn = ref(false);

const member = ref({
  email: '',
  password: '',
  name: '',
  region_1depth_name: '',
  cellphone: '',
});

const signup = async () => {
  const isEmailDuplicated = await memberStore.duplicatedEmail(
    member.value.email
  );

  if (isEmailDuplicated) {
    alert('이미 가입된 이메일입니다. 다른 이메일을 사용해 주세요.');
    return;
  }

  // 중복이 없는 경우 회원가입 진행
  const res = await memberStore.signup(member.value);
  if (res) {
    // 가입 성공 시
    alert('이메일에 발송된 링크를 눌러 가입을 완료하세요!');
    router.push('/login');
  } else {
    // 가입 실패 시
    alert('가입에 실패했습니다. 다시 시도해 주세요.');
  }
};
</script>

<style>
.uBtn {
  display: block;
  width: 100%;
  height: 50px;
  padding-bottom: 1px;
  font-size: 15px;
  color: #000;
  background: #fff;
  border: 1px solid #ccc;
  border-radius: 10px;
  cursor: pointer;
}

.uBtn:disabled {
  color: #fff;
  font-weight: bold;
  background: #ccc;
  border: 0;
}

.uBtn.active {
  color: #fff;
  font-weight: bold;
  background: #333;
  border: 0;
}

.uBtn.active:disabled {
  background: #ccc;
}

.uBtn.point {
  color: #fff;
  font-weight: bold;
  background: #1769ff;
  border: 0;
}

.uBtn.point:disabled {
  background: #ccc;
}

.uInputArea {
  display: table;
  width: 100%;
  height: 30px;
  padding: 10px 0;
  border-bottom: 1px solid #000;
}

.uInputArea.emailType {
  position: relative;
  /* background: url('//openimage.interpark.com/member/common/icon/icon_arrow_down.png')
    100% 50% no-repeat; */
  background-size: 22px auto;
}

.uInputArea.emailType .uInput .inputBox .inputText {
  padding-right: 0;
}

.uInputArea.emailType .uSelectBox label {
  min-width: 106px;
  background: none;
}

.uInputArea.emailType .uSelectBox select {
  padding-left: 0;
}

.uInputArea.emailType.active .inputBox {
  padding-right: 60px;
}

.uInputArea.emailType.active .selectCol {
  position: absolute;
  top: 50%;
  right: 0;
  padding: 0;
  opacity: 0;
  margin-top: -15px;
}

.uInputArea.emailType.active .selectCol label {
  min-width: 0;
  width: 50px;
  height: 100%;
  padding: 0;
}

@media screen and (min-width: 769px) {
  .uInputArea.emailType.active .selectCol label {
    min-width: 130px;
    width: auto;
    height: 100%;
    padding: 0;
  }
}

.uInputArea .col {
  display: table-cell;
  vertical-align: top;
}

.uInputArea .col:first-child {
  width: 100%;
  padding-left: 5px;
}

.uInputArea .col:last-child {
  padding-right: 5px;
}

.uInputArea .label {
  display: inline-block;
  height: 30px;
  padding: 0;
  line-height: 30px;
}

.uInputArea .uInput:after {
  content: '';
  display: block;
  clear: both;
}

.uInputArea .uInput label {
  font-size: 15px;
  line-height: 30px;
  color: #000;
  float: left;
  font-weight: bold;
}

.uInputArea .uInput .inputBox {
  overflow: hidden;
  position: relative;
}

.uInputArea .uInput .inputBox .inputText {
  font-size: 15px;
  line-height: 30px;
  color: #000;
  display: block;
  width: 100%;
  height: 30px;
  padding: 0 25px 0 10px;
  box-sizing: border-box;
  border: 0;
  outline: 0;
  background: none;
}

.uInputArea .uInput .inputBox .inputText:-webkit-autofill,
.uInputArea .uInput .inputBox .inputText:-webkit-autofill:hover,
.uInputArea .uInput .inputBox .inputText:-webkit-autofill:focus,
.uInputArea .uInput .inputBox .inputText:-webkit-autofill:active {
  -webkit-box-shadow: 0 0 0 30px #fff inset !important;
}

.uInputArea .uInput .inputBox .inputText::placeholder {
  font-size: 15px;
  line-height: 30px;
  color: #000;
  color: #ccc;
}

.uInputArea .uInput .inputBox .btnDel {
  display: none;
  position: absolute;
  bottom: 0;
  right: 0;
  width: 25px;
  height: 30px;
}

.uInputArea .uInput .inputBox .btnDel:after {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 14px;
  height: 14px;
  margin: -7px 0 0 -7px;
  background: url('//openimage.interpark.com/member/common/icon/icon_delete.png')
    0 0 no-repeat;
  background-size: 100% auto;
  content: '';
}

.uInputArea .uBtnArea {
  white-space: nowrap;
  font-size: 0;
}

.uInputArea .uBtnArea .uBtn {
  display: inline-block;
  width: auto;
  font-size: 13px;
  height: 30px;
  margin-left: 5px;
  color: #666;
  border: 0;
}

.uInputArea .uBtnArea .uBtn:first-child {
  margin-left: 0;
}

.uInputArea .uBtnArea .uBtn.borderType {
  min-width: 70px;
  padding-left: 10px;
  padding-right: 10px;
  box-sizing: border-box;
  border-radius: 6px;
  border: 1px solid #ccc;
  color: #000;
}

.ErrorMessage {
  font-size: 13px;
  line-height: 20px;
  color: #dc941b;
  margin-top: 5px;
  padding: 0 5px;
}

.uErrorText {
  font-size: 13px;
  line-height: 20px;
  color: #dc941b;
  margin-top: 5px;
  padding: 0 5px;
}

.uNoticeText {
  font-size: 13px;
  line-height: 20px;
  color: #999;
  margin-top: 5px;
  padding: 0 5px;
}

.uNoticeText.point {
  color: #dc941b;
}

.businessJoinNotice {
  font-size: 13px;
  line-height: 20px;
  color: #999;
  margin-top: 13px;
  padding: 0 5px;
}

.businessJoinNotice p {
  position: relative;
  padding-left: 8px;
}

.businessJoinNotice p::before {
  position: absolute;
  top: 8px;
  left: 0;
  display: inline-block;
  content: '';
  width: 3px;
  height: 3px;
  background-color: #999;
  border-radius: 100%;
}

.businessJoinNotice p a {
  color: #999;
  text-decoration: underline;
}

.uCheckbox {
  font-size: 0;
}

.uCheckbox:after {
  content: '';
  display: block;
  clear: both;
}

.uCheckbox.sType label {
  min-height: 23px;
}

.uCheckbox.sType .text {
  padding-top: 2px;
}

.uCheckbox.sType .text:after {
  width: 23px;
  height: 23px;
}

.uCheckbox input[type='checkbox'] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
  pointer-events: none;
}

.uCheckbox input[type='checkbox']:checked + .text:after {
  border-color: #1769ff;
  background-color: #1769ff;
  background-position: 0 100%;
}

.uCheckbox label {
  float: left;
  min-height: 28px;
}

.uCheckbox .text {
  font-size: 13px;
  line-height: 20px;
  color: #000;
  display: inline-block;
  position: relative;
  height: 100%;
  padding-top: 4px;
  padding-left: 33px;
  cursor: pointer;
}

.uCheckbox .text:after {
  background: url('//openimage.interpark.com/member/common/icon/icon_check_new.png')
    0 0 no-repeat;
  background-size: 100% auto;
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 28px;
  height: 28px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  background-color: #fff;
  background-position: 0 0;
  background-size: 100% auto;
  border-radius: 50%;
}

.uCheckbox .checkboxCont {
  overflow: hidden;
}

.uCheckbox .checkboxCont .uNoticeText {
  padding: 0;
}

.uCheckbox .btnLink {
  font-size: 15px;
  line-height: 20px;
  color: #666;
  display: inline-block;
  position: relative;
  padding-right: 20px;
  vertical-align: top;
}

.uCheckbox .btnLink:after {
  position: absolute;
  top: 50%;
  height: 15px;
  margin-top: -7.5px;
  background: url('//openimage.interpark.com/member/common/icon/icon_arrow_left.png')
    0 0 no-repeat;
  background-size: 100% auto;
  content: '';
  width: 15px;
  right: 0;
}

.uCheckbox .btnLink.hasSmallLink:after {
  display: none;
}

.uCheckbox .btnLink em {
  font-size: 13px;
  line-height: 20px;
  color: #666;
  position: relative;
  display: inline-block;
  width: auto;
  margin-left: 10px;
}

.uCheckbox .btnLink em:after {
  position: absolute;
  top: 50%;
  height: 14px;
  margin-top: -7px;
  background: url('//openimage.interpark.com/member/common/icon/icon_arrow_left.png')
    0 0 no-repeat;
  background-size: 100% auto;
  background-position: 50% 50%;
  content: '';
  width: 15px;
  right: -14px;
}

@media screen and (max-width: 420px) {
  .uCheckbox .btnLink em {
    margin-top: 10px;
    margin-left: 0;
  }
}

.label {
  font-size: 15px;
  line-height: 22px;
  color: #000;
  padding: 10px 0;
  font-weight: bold;
}

.uRadioBox {
  overflow: hidden;
  height: 40px;
  padding: 2px;
  box-sizing: border-box;
  border-radius: 10px;
  background: #f3f3f3;
}

.uRadioBox input[type='radio'] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
  pointer-events: none;
}

.uRadioBox input[type='radio']:checked + .text {
  background: #fff;
  color: #000;
  border-radius: 8px;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.3);
}

.uRadioBox .uRadio {
  font-size: 15px;
  line-height: 36px;
  color: #666;
  float: left;
  width: 50%;
  height: 100%;
  background: #f3f3f3;
  text-align: center;
}

.uRadioBox .uRadio label,
.uRadioBox .uRadio .text {
  display: block;
  position: relative;
  height: 100%;
}

/* .uSelectBox label {
  display: inline-block;
  position: relative;
  min-width: 160px;
  height: 30px;
  vertical-align: top;
  background: url('//openimage.interpark.com/member/common/icon/icon_arrow_down.png')
    100% 50% no-repeat;
  background-size: 22px auto;
}

.uSelectBox label select {
  padding: 0 14px 0 10px;
  height: 100%;
  border: 0;
  background: none;
  outline: 0;
  width: 100%;
  -webkit-appearance: none;
  appearance: none;
  font-size: 15px;
  line-height: 30px;
  color: #666;
} */

button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
  padding: 0;
  border: none;
  outline: none;
  background: transparent;
  -webkit-user-select: none;
  user-select: none;
  font-family: -apple-system, BlinkMacSystemFont, Roboto, 'Droid Sans',
    'Helvetica Neue', 'Apple SD Gothic Neo', 'sans-serif', sans-serif;
}

button:active {
  opacity: 0.5;
}

select::-ms-expand {
  display: none;
}

.contentWrapper {
  padding: 0 20px;
}

@media screen and (min-width: 769px) {
  .contentWrapper {
    width: 500px;
    min-height: 800px;
    box-sizing: border-box;
    margin: 0 auto;
    padding: 0;
  }
}

.registContent {
  padding-top: 15px;
  padding-bottom: 40px;
}

@media screen and (min-width: 769px) {
  .registContent {
    padding-top: 75px;
  }
}

.registContent .uBlock {
  min-height: 76px;
}

.registContent .uBlock.checkBlock {
  min-height: 0;
  margin-top: 15px;
}

.registContent .uBlock.validPeriod {
  padding-bottom: 20px;
}

.registContent .uBlock.validPeriod .label {
  padding-left: 5px;
}

.registContent .uBlock .uCheckbox {
  margin-top: 10px;
}

.registContent .uBlock .uCheckbox:first-child {
  margin-top: 0;
}

.registContent .uBlock .uErrorText {
  display: none;
}

.registContent .uBlock .certifyTime {
  font-size: 13px;
  line-height: 20px;
  color: #dc941b;
  margin-top: 5px;
  padding: 0 5px;
  font-weight: bold;
}

.registContent .uBlock .blockText {
  padding: 20px 0;
  font-size: 15px;
  line-height: 22px;
}

.registContent .agreeBlock {
  min-height: auto;
}

.registContent .agreeBlock .uInputArea {
  border: 0;
}

.registContent .ubtnArea {
  margin-top: 45px;
}

.registContent .infoBlock {
  min-height: 0;
  padding: 0 5px 30px;
}

.registContent .infoList li {
  margin-top: 10px;
  font-size: 17px;
  line-height: 25px;
}

.registContent .infoList li:after {
  content: '';
  display: block;
  clear: both;
}

.registContent .infoList li:first-child {
  margin-top: 0;
}

.registContent .infoList li .title {
  float: left;
  padding-right: 10px;
  font-weight: bold;
}

.registContent .infoList li .text {
  overflow: hidden;
}

.registContent .accountValiBlock {
  margin-top: 25px;
  padding: 0 0 25px 5px;
}

.registContent .accountValiBlock .accountGuide {
  font-size: 13px;
  line-height: 20px;
  color: #dc941b;
  font-weight: bold;
}

.registContent .accountValiBlock .blockText {
  font-size: 13px;
  line-height: 20px;
  color: #000;
  padding: 10px 0;
}

.registContent .accountValiBlock .btnArrow {
  font-size: 13px;
  line-height: 20px;
  color: #000;
  display: inline-block;
  position: relative;
  padding-right: 8px;
  border-bottom: 1px solid #000;
}

.registContent .accountValiBlock .btnArrow:after {
  position: absolute;
  top: 50%;
  right: 0;
  width: 5px;
  height: 8px;
  margin-top: -4px;
  background: url('//openimage.interpark.com/member/common/icon/icon_btn_arrow.png')
    0 0 no-repeat;
  background-size: 100% auto;
  content: '';
}

.completeContent {
  padding-bottom: 247px;
}

@media screen and (min-width: 769px) {
  .completeContent {
    padding-top: 24px;
  }

  .completeContent .title br {
    display: none;
  }
}

.completeContent .title {
  font-size: 24px;
  line-height: 37px;
  color: #000;
  margin-top: 120px;
  font-weight: bold;
  text-align: center;
}

.completeContent .text {
  font-size: 15px;
  line-height: 22px;
  color: #666;
  margin-top: 24px;
  text-align: center;
}

.completeContent .btnArea {
  display: table;
  width: 100%;
  margin-top: 30px;
}

.completeContent .btnArea .col {
  display: table-cell;
  width: 50%;
  padding: 0 5px;
}

.completeContent .btnArea .col:first-child {
  padding-left: 0;
}

.completeContent .btnArea .col:last-child {
  padding-right: 0;
}

.completeContent .bannerArea {
  margin-top: 30px;
}

.completeContent .bannerArea a {
  display: block;
}

.completeContent .bannerArea img {
  width: 100%;
}

.guideContent {
  padding-bottom: 247px;
}

@media screen and (min-width: 769px) {
  .guideContent .title br {
    display: none;
  }
}

.guideContent .title {
  font-size: 20px;
  line-height: 30px;
  color: #000;
  margin-top: 60px;
  font-weight: bold;
  text-align: center;
}

.guideContent .title span {
  display: block;
}

.guideContent .phrase {
  font-size: 15px;
  line-height: 22px;
  color: #666;
  margin-top: 24px;
  text-align: left;
}

.guideContent .phrase em {
  font-weight: bold;
}

.guideContent .btnArea {
  display: table;
  width: 100%;
  margin-top: 60px;
}

.guideContent .btnArea .col {
  display: table-cell;
  width: 50%;
  padding: 0 5px;
}

.guideContent .btnArea .col:first-child {
  padding-left: 0;
}

.guideContent .btnArea .col:last-child {
  padding-right: 0;
}

.guideContent .bannerArea {
  margin-top: 30px;
}

.guideContent .bannerArea a {
  display: block;
}

.guideContent .bannerArea img {
  width: 100%;
}

.guideContent .termsAgreeWrap .allAgree {
  padding-top: 24px;
  margin-top: 24px;
  border-top: 1px solid #ddd;
}

.guideContent .termsAgreeWrap .allAgree .text {
  font-weight: bold;
  font-size: 17px;
  line-height: 25px;
  color: #000;
}

.guideContent .termsAgreeWrap .termsBlock {
  margin-top: 10px;
  padding-left: 5px;
}

.guideContent .termsAgreeWrap .termsBlock .uCheckbox + .uCheckbox {
  margin-top: 15px;
}

.guideContent .termsAgreeWrap + .ubtnArea {
  margin-top: 45px;
}

.termsContent {
  padding-top: 25px;
  padding-bottom: 40px;
}

@media screen and (min-width: 769px) {
  .termsContent {
    padding-top: 75px;
  }
}

.termsContent .text {
  font-size: 15px;
  line-height: 22px;
  color: #666;
}

.termsContent .text + .termsAgreeWrap {
  margin-top: 45px;
}

.termsContent .uBlock {
  margin-top: 10px;
}

.termsContent .uBlock:first-child {
  margin-top: 0;
}

.termsContent .termsBlock {
  margin-top: 10px;
  padding-left: 5px;
}

.termsContent .termsBlock + .termsBlock {
  margin-top: 20px;
}

.termsContent .allAgree .text {
  font-size: 17px;
  line-height: 25px;
  color: #000;
  padding-top: 0;
  font-weight: bold;
}

.termsContent .termsItem .uCheckbox {
  margin-top: 15px;
}

.termsContent .termsItem .uCheckbox:first-child {
  margin-top: 0;
}

.termsContent .ubtnArea {
  margin-top: 45px;
}

.termsContent .termsDetailCont {
  margin: 15px 5px 0 0;
  padding: 0 15px;
  background: #f3f3f3;
  border-radius: 10px;
  display: none;
}

.termsContent .termsCont {
  padding: 15px 0;
}

.termsContent .termsCont:first-child {
  border-top: 0;
}

.termsContent .termsCont .title {
  font-size: 13px;
  line-height: 20px;
  color: #666;
}

.termsContent .termsCont .checkList {
  margin-top: 5px;
}

.termsContent .termsCont .checkList:after {
  content: '';
  display: block;
  clear: both;
}

.termsContent .termsCont .checkList li {
  float: left;
  width: 50%;
  margin-top: 5px;
}

.termsContent .termsCont .checkList + .uCheckbox {
  margin-top: 15px;
  margin-bottom: 10px;
  padding-top: 15px;
  border-top: 1px solid #ccc;
}

.termsContent .termsCont .uCheckbox .text {
  color: #666;
}

.termsContent .termsCont .uCheckbox + .title {
  margin-top: 20px;
}

.termsContent .termsCont .termsText {
  font-size: 11px;
  line-height: 17px;
  color: #999;
}

.termsContent .termsCont .termsText ul {
  margin-top: 10px;
}

.termsContent .termsCont .termsText li {
  margin-top: 10px;
}

.termsContent .businessCertifyArea {
  padding: 10px 0 40px;
}

.termsContent .businessCertifyArea .label {
  font-size: 17px;
  line-height: 25px;
  color: #000;
  padding-left: 5px;
}

.termsContent .termsPart {
  margin-top: 8px;
}

.termsContent .termsPart li a {
  font-size: 13px;
  color: #999;
}

.termsContent .termsPart li a.btnLink {
  padding-right: 15px;
}

.termsContent .termsPart li a.btnLink:after {
  width: 10px;
  margin-top: -5px;
}

.termsContent .choiceAgree .uCheckbox .checkboxCont > .btnLink {
  padding-right: 0;
}

.termsContent .choiceAgree .uCheckbox .checkboxCont > .btnLink:after {
  content: none;
}

abbr,
address,
article,
aside,
audio,
b,
blockquote,
body,
canvas,
caption,
cite,
code,
dd,
del,
details,
dfn,
div,
dl,
dt,
em,
fieldset,
figcaption,
figure,
footer,
form,
h1,
h2,
h3,
h4,
h5,
h6,
header,
hgroup,
html,
i,
iframe,
img,
ins,
kbd,
label,
legend,
li,
mark,
menu,
nav,
object,
ol,
p,
pre,
q,
samp,
section,
small,
span,
strong,
sub,
summary,
sup,
table,
tbody,
td,
tfoot,
th,
thead,
time,
tr,
ul,
var,
video {
  margin: 0;
  padding: 0;
  border: 0;
  outline: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
}

article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}

ol,
ul {
  list-style: none;
}

blockquote,
q {
  quotes: none;
}

blockquote:after,
blockquote:before,
q:after,
q:before {
  content: '';
  content: none;
}

dfn,
em,
i {
  font-style: normal;
}

a {
  margin: 0;
  padding: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
  text-decoration: none;
}

ins {
  text-decoration: none;
}

ins,
mark {
  background-color: #ff9;
  color: #000;
}

mark {
  font-style: italic;
  font-weight: 700;
}

del {
  text-decoration: line-through;
}

table {
  border-collapse: collapse;
  border-spacing: 0;
}

hr {
  display: none;
}

input,
select {
  vertical-align: middle;
}

input {
  -webkit-margin-before: 0;
  -webkit-margin-after: 0;
}

input[type='checkbox'] {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}

input[type='text']::-ms-clear {
  display: none;
  width: 0;
  height: 0;
}

input[type='text']::-ms-reveal {
  display: none;
  width: 0;
  height: 0;
}

input[type='search']::-webkit-search-cancel-button,
input[type='search']::-webkit-search-decoration,
input[type='search']::-webkit-search-results-button,
input[type='search']::-webkit-search-results-decoration {
  display: none;
}

html {
  text-size-adjust: none;
  font-size: 62.5%;
}

body {
  font-size: 1.5rem;
  font-family: Pretendard, -apple-system, BlinkMacSystemFont, system-ui, Roboto,
    Helvetica Neue, Segoe UI, Apple SD Gothic Neo, Malgun Gothic,
    Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol, sans-serif;
  font-weight: 400;
  color: #000;
}

a,
button {
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

input[type='text'] {
  font-family: Pretendard, -apple-system, BlinkMacSystemFont, system-ui, Roboto,
    Helvetica Neue, Segoe UI, Apple SD Gothic Neo, Malgun Gothic,
    Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol, sans-serif;
}

select {
  font-family: Pretendard, -apple-system, BlinkMacSystemFont, system-ui, Roboto,
    Helvetica Neue, Segoe UI, Apple SD Gothic Neo, Malgun Gothic,
    Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol, sans-serif;
}

.blind {
  position: absolute;
  overflow: hidden;
  clip: rect(0 0 0 0);
  margin: -0.1rem;
  width: 0.1rem;
  height: 0.1rem;
}

.header {
  position: relative;
  height: 50px;
  padding-top: 5px;
  line-height: 44px;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  text-align: center;
}

.header .headerInner {
  position: relative;
  width: 100%;
  height: 100%;
}

.header .headerLogo {
  background: url(https://daqu2024-s3.s3.ap-northeast-2.amazonaws.com/tiggle.png)
    0 0 no-repeat;
  background-size: 100% auto;
  position: absolute;
  top: 12px;
  left: 20px;
  width: 119px;
  height: 30px;
}

.header .headerTitle {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  height: 100%;
  font-size: 15px;
  padding: 0 150px;
  line-height: 50px;
}

@media screen and (min-width: 769px) {
  .header {
    height: 88px;
    padding-top: 0;
    line-height: 68px;
    border-bottom: 1px solid rgba(0, 0, 0, 0.08);
  }

  .header .headerInner {
    max-width: 1280px;
    margin: 0 auto;
  }

  .header .headerLogo {
    background-position: 0 50%;
  }

  .header .headerLogo,
  .header .headerLogo.blackLogo {
    position: absolute;
    top: auto;
    bottom: 20px;
    left: 24px;
    width: 151px;
    height: 38px;
  }

  .header .headerLogo.blackLogo {
    background: url(//openimage.interpark.com/member/common/logo_interpark.svg)
      0 0 no-repeat;
    background-size: 100% auto;
  }

  .header .headerTitle {
    padding: 0 190px;
    line-height: 88px;
  }
}

.footer {
  height: 57px;
}

.footer .copyright {
  text-align: center;
  padding: 20px 0;
  font-size: 11px;
  line-height: 17px;
  color: #999;
}

@media screen and (min-width: 769px) {
  .footer {
    height: 80px;
    background: #fafafa;
  }

  .footer .copyright {
    padding: 0;
    line-height: 80px;
  }
}

.registContent .uBlock {
  min-height: 76px;
}
abbr,
address,
article,
aside,
audio,
b,
blockquote,
body,
canvas,
caption,
cite,
code,
dd,
del,
details,
dfn,
div,
dl,
dt,
em,
fieldset,
figcaption,
figure,
footer,
form,
h1,
h2,
h3,
h4,
h5,
h6,
header,
hgroup,
html,
i,
iframe,
img,
ins,
kbd,
label,
legend,
li,
mark,
menu,
nav,
object,
ol,
p,
pre,
q,
samp,
section,
small,
span,
strong,
sub,
summary,
sup,
table,
tbody,
td,
tfoot,
th,
thead,
time,
tr,
ul,
var,
video {
  margin: 0;
  padding: 0;
  border: 0;
  outline: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
}
user agent stylesheet div {
  display: block;
  unicode-bidi: isolate;
}
body {
  font-size: 1.5rem;
  font-family: Pretendard, -apple-system, BlinkMacSystemFont, system-ui, Roboto,
    Helvetica Neue, Segoe UI, Apple SD Gothic Neo, Malgun Gothic,
    Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol, sans-serif;
  font-weight: 400;
  color: #000;
}
html {
  text-size-adjust: none;
  font-size: 62.5%;
}

.registContent .uBlock {
  min-height: 76px;
}
</style>
