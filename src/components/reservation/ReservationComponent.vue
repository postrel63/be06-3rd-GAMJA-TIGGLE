<template>
  <div id="cont_wrap" class="clear_g">
    <div id="conts_section" class="pr_none">
      <!-- contents -->
      <div id="conts" class="clear_g">
        <!-- 컨텐츠 영역 시작 -->
        <h2 class="screen_out">마이티켓</h2>
        <!-- 구매 티켓 정보 -->
        <div class="wrap_ticket">
          <div class="box_ticket">
            <div class="img">
              <img
                id="imgPerfImgUrl"
                onerror="noImage(this,180,254)"
                alt=""
                :src="programStore.myReservation.imageFiles[0]"
              />
            </div>
            <div class="infor_text">
              <span class="ico_list_b ico_list_b1" id="iconType" style=""
                >좌석우위</span
              >
              <p class="ticket_title">
                <a
                  ><span id="txtPerfMainName">
                    {{ programStore.myReservation.programName }}</span
                  ></a
                >
              </p>
            </div>
            <div class="infor_text_check">
              <dl>
                <dt>예매번호</dt>
                <dd>
                  <span class="fc_green" id="txtRsrvNo">{{
                    programStore.myReservation.ticketNumber
                  }}</span>
                </dd>
                <dt id="dtPlaceName">공연장</dt>
                <dd>
                  <a
                    class="theater"
                    id="txtPlaceName"
                    :title="programStore.myReservation.locationName"
                    ><span class="place">{{
                      programStore.myReservation.locationName
                    }}</span
                    ><em></em
                  ></a>
                </dd>
              </dl>
              <dl>
                <dt>예매일시</dt>
                <dd id="txtRsrvDate">
                  {{ formatDate(programStore.myReservation.createdAt) }}
                </dd>
                <dt id="titleMemberName">예매자</dt>
                <dd id="txtMemberName">
                  {{ programStore.myReservation.name }}
                </dd>
              </dl>
              <dl>
                <dt>관람일시</dt>
                <dd id="txtScheduleDate">
                  {{ formatDateTime(programStore.myReservation.date) }}
                </dd>
                <dt id="titlePayStatus">상태</dt>
                <dd id="txtPayStatus">
                  {{
                    programStore.myReservation.status == 'COMPLETED'
                      ? '예매 완료'
                      : '환불 완료'
                  }}
                </dd>
              </dl>
              <dl>
                <dt>매수</dt>
                <dd id="txtRsrvVolume">1매</dd>
                <template
                  v-if="programStore.myReservation.status == 'COMPLETED'"
                >
                  <dt id="titleCancelDt">취소가능</dt>
                  <dd id="txtCancelDt">
                    {{ formatRefundDate(programStore.myReservation.date) }}
                    17:00 까지
                  </dd>
                </template>
              </dl>
            </div>
          </div>
          <div class="ticket_cancelok" id="divPayArea" style="display: none">
            <div class="cancelok_comm">
              <p class="fc_green2" id="infoPayArea"></p>
            </div>
            <div class="btn_ticket" id="btnPayArea" style="display: none">
              <span class="button btColorGreen"
                ><a href="javascript:goPayment();" class="btSizeL"
                  >결제하기</a
                ></span
              >
            </div>
          </div>
        </div>
        <div class="wrap_get" id="wrap_get">
          <h2 class="tit_sub_float">티켓수령방법</h2>
          <table class="tbl tbl_style01" summary="">
            <caption class="hide"></caption>
            <tbody>
              <tr>
                <th class="fst">수령방법</th>
                <td id="txtDelvyType" class="fst">
                  현장수령 | 공연 당일 현장 교부처에서 예매번호 및 본인 확인 후
                  티켓을 수령하여 입장이 가능합니다.
                </td>
              </tr>
              <tr class="event_delvy_type_dv003" style="display: none">
                <th>받으시는 분</th>
                <td id="txtDelvyName"></td>
              </tr>
              <tr class="event_delvy_type_dv003" style="display: none">
                <th>휴대폰 번호</th>
                <td id="txtDelvyTn"></td>
              </tr>
              <tr class="event_delvy_type_dv003" style="display: none">
                <th>주소</th>
                <td>
                  <span id="txtDelvyAddr"></span>
                  <input type="hidden" id="delvyOrderNo" value="" />
                  <input type="hidden" id="recvZipno" value="" />
                  <input type="hidden" id="recvAddr" value="" />
                  <input type="hidden" id="recvAddrDtl" value="" />
                  <a
                    href="javascript: popChangeAddr();"
                    class="btn_flexible btn_arr"
                    id="btnChangeAddr"
                    style="display: none"
                    ><span>배송주소 변경</span></a
                  >
                </td>
              </tr>
              <tr class="event_delvy_type_dv004" style="display: none">
                <th>배송 메시지</th>
                <td id="txtDelvyMemo"></td>
              </tr>
              <input
                type="hidden"
                id="parcelRecvYn"
                name="parcelRecvYn"
                value=""
              />
              <tr class="event_delvy_type_dv003" style="display: none">
                <th>송장번호</th>
                <td>
                  <span id="txtTranspNo"></span>
                  <a
                    href="javascript: popDelvyTrace()"
                    class="btn_flexible btn_arr"
                    id="btnDelvyTrace"
                    style="display: none"
                    ><span>배송 추적</span></a
                  >
                  <input type="hidden" id="transpNo" value="" />
                  <input type="hidden" id="delvyTrsfYn" value="" />
                  <input type="hidden" id="delvyBp" value="" />
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <!-- //티켓 수령방법 -->

        <!-- 결제 내역 -->
        <div class="wrap_payment" id="wrap_payment" style="">
          <h2 class="tit_sub_float">결제 내역</h2>
          <div class="ctr_detail">
            <a
              href="javascript: goRsrvModify('PAY');"
              class="btn_flexible btn_tB_radius btn_edit_rsrv"
              id="btnEditPayMethod"
              style=""
              ><span>결제수단 변경</span></a
            >
          </div>
          <table class="tbl tbl_style01" summary="">
            <tbody>
              <tr class="event_pay_method_pay" style="">
                <th>결제방법</th>
                <td id="txtPayMethod">신용카드</td>
              </tr>
              <tr class="event_pay_method_card">
                <th>카드명</th>
                <td id="kcpCardInfo">신한카드</td>
              </tr>
              <tr class="event_pay_method_card">
                <th>할부</th>
                <td id="kcpCardQuota">일시불</td>
              </tr>
              <tr class="event_pay_method_vbank" style="">
                <th>예금주명</th>
                <td id="depositName">멜론티켓</td>
              </tr>
              <tr class="event_pay_method_vbank" style="">
                <th>입금상태</th>
                <td id="depositStatus">결제 완료</td>
              </tr>
              <tr class="event_pay_method_mobile" style="display: none">
                <th>휴대폰 번호</th>
                <td id="tel"></td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- 좌석정보  -->
        <div class="wrap_seat">
          <h2 class="tit_sub_float" id="titleSeatInfo">좌석정보(1매)</h2>
          <!-- @@@ 2016.02.23 매수추가 -->
          <form id="dtlForm" method="get" name="dtlForm">
            <div class="box_seat">
              <table class="tbl tbl_style03" summary="">
                <caption class="hide"></caption>
                <colgroup>
                  <col width="12%" />
                  <col width="17%" />
                  <col width="" />
                  <col width="14%" />
                  <col width="20%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>좌석등급</th>
                    <th>좌석번호</th>
                    <th>가격등급</th>
                    <th>구매</th>
                    <th>예매/취소</th>
                  </tr>
                </thead>
                <tbody id="seatTable">
                  <tr>
                    <td class="fst lst">
                      {{ programStore.myReservation.gradeName }}석
                    </td>
                    <td class="seat_site fst lst">
                      {{ programStore.myReservation.seatInfo }}
                    </td>
                    <td class="seat_infor fst lst">기본가</td>
                    <td class="seat_price fst lst">
                      {{
                        formatNumber(programStore.myReservation.totalPrice)
                      }}원
                    </td>
                    <td class="seat_cancle fst lst">
                      <label for="cancelok">취소 가능 </label>
                    </td>
                  </tr>
                </tbody>
              </table>
              <div class="seat_condition">
                <div class="condition_com">
                  <p class="con1" id="txtCancelCloseDtKr">
                    {{
                      formatRefundStringDate(programStore.myReservation.date)
                    }}
                    17시 00분 까지 예매취소가능(취소수수료 : 티켓금액의
                    <em>30%</em>)
                  </p>
                  <p class="con2" id="txtCancelCloseDtMsg" style="">
                    * 예매일 이후 취소 시 예매수수료는 환불되지 않습니다. (단,
                    예매 당일 밤 12시 이전 취소 시에는 취소수수료 없음)
                  </p>
                </div>
                <div class="seat_condition_btn">
                  <span class="button btColorGreen" id="btnRsrvCancelPreview"
                    ><a class="btSizeL">예매취소 요청</a></span
                  >
                </div>
              </div>
            </div>
          </form>
        </div>

        <!-- 수수료 안내  -->
        <div class="wrap_commission" style="">
          <h2 class="tit_sub_float">취소 수수료안내</h2>
          <div class="box_commission">
            <table class="tbl tbl_style01" summary="">
              <colgroup>
                <col width="130" />
                <col width="" />
              </colgroup>
              <caption class="hide"></caption>
              <tbody>
                <tr id="feeInfoTitle">
                  <th class="fst">취소기간</th>
                  <td class="fst"><span>예매당일</span></td>
                  <td class="fst">
                    <span class="">관람일 9일 전 ~ 7일전</span>
                  </td>
                  <td class="fst">
                    <span class="">관람일 6일 전 ~ 3일전</span>
                  </td>
                  <td class="fst">
                    <span class="">관람일 2일 전 ~ 1일전</span>
                  </td>
                </tr>
                <tr id="feeInfoPeriod">
                  <th>취소일</th>
                  <td><span>2024.08.01</span></td>
                  <td><span class="">2024.08.02</span></td>
                  <td><span class="">2024.08.03 ~ 2024.08.06</span></td>
                  <td><span class="">2024.08.07 ~ 2024.08.08</span></td>
                </tr>
                <tr id="feeInfoPersent">
                  <th class="lst">취소 수수료</th>
                  <td class="lst"><span>없음</span></td>
                  <td class="lst"><span class="">티켓금액의 10%</span></td>
                  <td class="lst"><span class="">티켓금액의 20%</span></td>
                  <td class="lst"><span class="">티켓금액의 30%</span></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <!-- //수수료 안내  -->

        <!-- 유의사항  -->
        <div class="wrap_note">
          <h2 class="tit_sub_float">유의사항</h2>
          <div class="box_note">
            <ul class="dotlist4x4">
              <li>
                취소마감시간 이후 또는 관람일 당일 예매하신 건에 대해서는
                취소/변경/환불이 불가합니다.
              </li>
              <li>
                예매수수료는 예매 당일 밤 12시 이전까지 취소 시 환불 가능합니다.
              </li>
              <li>
                배송이 시작된 경우 취소마감시간이전까지 멜론티켓 고객센터로
                티켓을 반환해주셔야 환불이 가능하며, 도착한 일자 기준으로
                취소수수료가 부과됩니다.<br />
                (* 단, 반환된 티켓의 배송료는 환불되지 않으며 일괄배송 상품의
                경우 취소에 대한 자세한 문의는 고객센터로 문의해 주시기
                바랍니다.)
              </li>
              <li>
                지역 및 배송서비스 사정에 따라 배송사가 변경될 수 있으며,
                배송일이 추가적으로 소요될 수 있습니다. (CJ대한통운, 우체국 외
                1개 업체)
              </li>
              <li>
                일괄배송의 경우 공연 별로 배송일자가 상이하며 지정된 배송일자
                기준으로 배송이 시작됩니다.<br />
                (* 지정된 배송일자는 상세정보 및 예매공지사항에서 확인할 수
                있습니다.)
              </li>
              <li>
                예매한 모바일티켓을 타인에게 선물한 경우, 예매취소는 선물취소
                또는 선물거절 통해 티켓을 돌려받은 후에 가능합니다.
              </li>
              <li>
                예매취소 시점과 결제 시 사용하신 신용카드사의 환불 처리기준에
                따라 취소금액의 환급방법과 환급일은 다소 차이가 있을 수
                있습니다.
              </li>
              <li>
                신용카드 할부결제로 구매하신 과티켓 수량의 일부를 취소하실 경우,
                신용카드사의 사정에 따라 혜택(무이자 할부 등)의 적용 여부가
                달라질 수 있음을 유의하시기 바랍니다.
              </li>
              <li>
                기타 문의사항은 이용안내를 참조하시거나 고객센터 1899-0042 혹은
                1:1문의를 이용하시기 바랍니다.
              </li>
            </ul>
          </div>
        </div>
        <!-- //유의사항  -->
      </div>
      <!-- //contents -->
    </div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';
import { useProgramsStore } from '@/stores/useProgramsStore';
import { useRoute } from 'vue-router';
import {
  formatDate,
  formatDateTime,
  formatRefundDate,
  formatRefundStringDate,
} from '@/utils/formatDate';

import { formatNumber } from '@/utils/formatPrice';

const programStore = useProgramsStore();
const route = useRoute();

const id = route.params.id;

onMounted(async () => {
  await programStore.getMyReservation(id);
});
</script>

<style scoped>
#conts {
  display: block;
  position: relative;
  width: 1008px;
  min-height: 600px;
  padding: 0 20px 53px;
  margin: 0 auto;
}
.none,
.screen_out {
  overflow: hidden;
  font-size: 0 !important;
  width: 0 !important;
  height: 0 !important;
  line-height: 0 !important;
  text-indent: -9999px;
}
.wrap_ticket {
  overflow: hidden;
  position: relative;
  margin-top: 40px;
  border: 1px solid #ddd;
}
.wrap_ticket .box_ticket {
  overflow: hidden;
  position: relative;
  min-height: 254px;
  margin: 29px 29px 29px;
  padding-left: 219px;
}
.wrap_ticket .box_ticket .img {
  position: absolute;
  left: 0;
  top: 0;
  width: 219px;
}
.wrap_ticket .box_ticket .img a {
  display: block;
  overflow: hidden;
  width: 180px;
}
.wrap_ticket .box_ticket .img img {
  width: 180px;
  vertical-align: top;
}
img {
  border: 0 none;
}
.wrap_ticket .box_ticket .infor_text {
  overflow: hidden;
  width: 725px;
}
.ico_list_b1 {
  background-color: #41d26b;
}
.ico_list_b {
  display: inline-block;
  overflow: hidden;
  height: 16px;
  padding: 3px 7px;
  font-size: 14px;
  line-height: 19px;
  color: #fff;
  text-align: center;
}
.wrap_ticket .box_ticket .infor_text .ticket_title a {
  font-size: 28px;
  line-height: 32px;
  color: #333;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
  text-decoration: none;
}
.btn_flexible,
.btn_flexible span {
  display: inline-block;
  background: url(//cdnticket.melon.co.kr/resource/image/web/common/btn_flexible_20180510.png)
    no-repeat;
}
.wrap_ticket .box_ticket .infor_text_check {
  margin-top: 42px;
}
.wrap_ticket .box_ticket .infor_text_check dt {
  display: inline-block;
  width: 62px;
  font-size: 14px;
  margin-bottom: 11px;
}
.wrap_ticket .box_ticket .infor_text_check dd {
  display: inline-block;
  width: 240px;
  font-size: 14px;
  margin-bottom: 11px;
  margin-right: 6px;
  vertical-align: top;
}
.fc_green {
  color: #00b523;
}
.wrap_ticket .box_ticket .infor_text_check dd .theater {
  text-decoration: underline;
  vertical-align: top;
}
.wrap_ticket .box_ticket .infor_text_check dd .place {
  display: inline-block;
  overflow: hidden;
  max-width: 225px;
  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}
.wrap_ticket .box_ticket .infor_text_check dt {
  display: inline-block;
  width: 62px;
  font-size: 14px;
  margin-bottom: 11px;
}
.wrap_ticket .ticket_cancelok {
  position: relative;
  width: 100%;
  border-top: 1px solid #eee;
  padding: 22px 0 22px;
}
.wrap_ticket .ticket_cancelok .cancelok_comm {
  width: 730px;
}
.wrap_ticket .ticket_cancelok p {
  text-align: center;
  font-size: 20px;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
}
.wrap_ticket .ticket_cancelok .btn_ticket {
  position: absolute;
  top: 11px;
  right: 30px;
}
.btColorGreen {
  border: 1px solid #41d26b;
  background: #41d26b;
}
.wrap_ticket .ticket_cancelok .btn_ticket .btSizeL {
  padding-left: 93px;
  padding-right: 93px;
}
.btColorGreen a.btSizeL {
  color: #fff;
}
.btSizeL {
  display: inline-block;
  height: 48px;
  line-height: 46px;
  font-size: 16px;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
}
.wrap_get {
  position: relative;
  margin-top: 22px;
}
.tit_sub_float {
  width: auto;
  height: auto;
  margin: 0 130px 0 4px;
  padding: 9px 0 10px;
  line-height: 1.8;
}
.tit_sub_float {
  display: block;
  width: 100%;
  height: 54px;
  margin-left: 4px;
  font-size: 20px;
  line-height: 54px;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
  color: #000;
  text-align: left;
}
.ctr_detail {
  position: absolute;
  top: 15px;
  right: 0;
}
.btn_tB_radius {
  display: inline-block;
  overflow: hidden;
  height: 32px;
  padding: 0 0 0 15px;
  background-position: left -640px;
}
.btn_flexible,
.btn_flexible span {
  display: inline-block;
  background: url(//cdnticket.melon.co.kr/resource/image/web/common/btn_flexible_20180510.png)
    no-repeat;
}
.ctr_detail .btn_tB_radius span {
  width: 90px;
}
.btn_tB_radius span {
  display: inline-block;
  overflow: hidden;
  height: 28px;
  padding: 4px 15px 0 0;
  background-position: right -640px;
  font-size: 15px;
  line-height: 23px;
  color: #666;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
  text-align: center;
  vertical-align: top;
}
table.tbl_style01 {
  width: 100%;
  border: 1px solid #eee;
  color: #666;
}
.tbl {
  border-collapse: collapse;
  border-spacing: 0;
}
table.tbl_style01 tbody th.fst {
  padding-top: 26px;
  padding-bottom: 12px;
}
table.tbl_style01 tbody th {
  width: 166px;
  padding: 10px 10px 9px 30px;
  background: #fafafa;
  font-size: 14px;
  text-align: left;
  font-weight: 400;
  vertical-align: top;
}
table.tbl_style01 tbody td.fst {
  padding-top: 27px;
}
table.tbl_style01 tbody td {
  padding: 10px 20px 9px 45px;
  font-size: 14px;
}
table.tbl_style01 tbody th {
  width: 166px;
  padding: 10px 10px 9px 30px;
  background: #fafafa;
  font-size: 14px;
  text-align: left;
  font-weight: 400;
  vertical-align: top;
}
.wrap_get table .btn_flexible {
  vertical-align: top;
  margin-top: -6px;
  margin-left: 5px;
}
.btn_arr {
  display: inline-block;
  overflow: hidden;
  height: 28px;
  padding: 0 24px 0 0;
  background-position: right -80px;
}
.btn_arr span {
  display: inline-block;
  overflow: hidden;
  height: 18px;
  padding: 5px 0 5px 13px;
  background-position: left -80px;
  font-size: 12px;
  line-height: 20px;
  color: #666;
  text-align: center;
  vertical-align: top;
}
.wrap_payment {
  position: relative;
  margin-top: 22px;
}
.ctr_detail {
  position: absolute;
  top: 15px;
  right: 0;
}
.btn_tB_radius {
  display: inline-block;
  overflow: hidden;
  height: 32px;
  padding: 0 0 0 15px;
  background-position: left -640px;
}
.ctr_detail .btn_tB_radius span {
  width: 90px;
}
.btn_tB_radius span {
  display: inline-block;
  overflow: hidden;
  height: 28px;
  padding: 4px 15px 0 0;
  background-position: right -640px;
  font-size: 15px;
  line-height: 23px;
  color: #666;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
  text-align: center;
  vertical-align: top;
}
table.tbl_style01 {
  width: 100%;
  border: 1px solid #eee;
  color: #666;
}
.tbl {
  border-collapse: collapse;
  border-spacing: 0;
}
table.tbl_style01 tbody th {
  width: 166px;
  padding: 10px 10px 9px 30px;
  background: #fafafa;
  font-size: 14px;
  text-align: left;
  font-weight: 400;
  vertical-align: top;
}
table.tbl_style01 tbody td {
  padding: 10px 20px 9px 45px;
  font-size: 14px;
}
table.tbl_style01 tbody th {
  width: 166px;
  padding: 10px 10px 9px 30px;
  background: #fafafa;
  font-size: 14px;
  text-align: left;
  font-weight: 400;
  vertical-align: top;
}
.wrap_seat {
  position: relative;
  margin-top: 22px;
}
.wrap_seat .ctr_detail2 {
  position: absolute;
  top: 15px;
  left: 140px;
}
.ctr_detail {
  position: absolute;
  top: 15px;
  right: 0;
}
.wrap_seat .box_seat {
  border: 1px solid #ddd;
}
table.tbl_style03 {
  width: 100%;
}
.tbl {
  border-collapse: collapse;
  border-spacing: 0;
}
.wrap_seat .box_seat table.tbl_style03 th {
  padding: 17px 0 15px;
  font-size: 14px;
}
table.tbl_style03 thead th {
  background: #fafafa;
  font-weight: 400;
  text-align: center;
}
.wrap_seat .box_seat table.tbl_style03 td.lst {
  padding-bottom: 20px;
}
.wrap_seat .box_seat table.tbl_style03 td.fst {
  padding-top: 20px;
}
.wrap_seat .box_seat table.tbl_style03 td {
  padding: 10px 0;
  font-size: 14px;
  text-align: center;
}
table.tbl_style03 tbody td {
  vertical-align: top;
}
.wrap_seat .box_seat table.tbl_style03 td.seat_site {
  text-align: left;
  padding-left: 32px;
}
.wrap_seat .box_seat table.tbl_style03 td.seat_infor {
  padding-left: 40px;
  text-align: left;
}
.wrap_seat .box_seat table.tbl_style03 td.seat_price {
  padding-right: 35px;
  text-align: right;
}
.wrap_seat .box_seat table.tbl_style03 td.seat_cancle {
  padding-left: 70px;
  text-align: left;
}
.wrap_seat .box_seat table.tbl_style03 td.seat_cancle label {
  padding-right: 5px;
}
.wrap_seat .box_seat .seat_condition {
  position: relative;
  border-top: 1px solid #eee;
  margin: 0 20px 20px;
}
.wrap_seat .box_seat .seat_condition .condition_com {
  display: block;
  width: 794px;
  padding: 22px 0 0;
}
.wrap_seat .box_seat .seat_condition .condition_com p.con1 {
  font-size: 20px;
  color: #00cd3c;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
  text-align: center;
}
.wrap_seat .box_seat .seat_condition .condition_com p.con2 {
  margin-top: 3px;
  font-size: 13px;
  color: #666;
  text-align: center;
}
.wrap_seat .box_seat .seat_condition .seat_condition_btn {
  position: absolute;
  top: 20px;
  right: 0;
}
.btColorGreen {
  border: 1px solid #41d26b;
  background: #41d26b;
}
.wrap_seat .box_seat .seat_condition .seat_condition_btn .btSizeL {
  width: 170px;
}
.btColorGreen a.btSizeL {
  color: #fff;
}
.btSizeL {
  display: inline-block;
  height: 48px;
  line-height: 46px;
  font-size: 16px;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
}
.wrap_commission {
  position: relative;
  margin-top: 22px;
}
.wrap_commission .box_commission table.tbl_style01 th {
  width: 100px;
  font-size: 13px;
  letter-spacing: 0;
}
table.tbl_style01 tbody th.fst {
  padding-top: 26px;
  padding-bottom: 12px;
}
table.tbl_style01 tbody th {
  width: 166px;
  padding: 10px 10px 9px 30px;
  background: #fafafa;
  font-size: 14px;
  text-align: left;
  font-weight: 400;
  vertical-align: top;
}
.wrap_commission .box_commission table.tbl_style01 td.fst {
  padding-top: 26px;
}
.wrap_commission .box_commission table.tbl_style01 td {
  padding: 10px 0 9px 10px;
  font-size: 13px;
  letter-spacing: 0;
}
table.tbl_style01 tbody td.fst {
  padding-top: 27px;
}
.wrap_commission .box_commission table.tbl_style01 td {
  padding: 10px 0 9px 10px;
  font-size: 13px;
  letter-spacing: 0;
}
table.tbl_style01 tbody th {
  width: 166px;
  padding: 10px 10px 9px 30px;
  background: #fafafa;
  font-size: 14px;
  text-align: left;
  font-weight: 400;
  vertical-align: top;
}
.wrap_commission .box_commission table.tbl_style01 td {
  padding: 10px 0 9px 10px;
  font-size: 13px;
  letter-spacing: 0;
}
.wrap_commission .box_commission table.tbl_style01 td.lst {
  padding-bottom: 26px;
  padding-top: 13px;
}
.wrap_note {
  position: relative;
  margin-top: 22px;
}
.wrap_note .box_note {
  background: #fafafa;
  padding: 29px 25px 20px;
}
.wrap_note .box_note .dotlist4x4 li {
  margin-bottom: 11px;
  color: #888;
}
.dotlist4x4 li {
  margin-bottom: 7px;
  padding: 0 0 0 13px;
  background: url(//cdnticket.melon.co.kr/resource/image/web/common/dot_4x4.png)
    no-repeat 0 7px;
  font-size: 13px;
  line-height: 18px;
}
</style>
