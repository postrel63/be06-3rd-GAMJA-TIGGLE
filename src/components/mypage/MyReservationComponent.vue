<template>
  <tbody id="rsrvTbody">
    <tr>
      <td class="lst">
        <p>
          {{ reservation.status == 'COMPLETED' ? '예매 완료' : '환불 완료' }}
        </p>
      </td>
      <td>
        <div class="movie_infor">
          <div class="thumb_90x125 img">
            <a>
              <img :src="reservation.imageFiles[0]" width="90" alt="" />
              <span class="frame"></span>
            </a>
          </div>
          <p class="infor_text">
            <span class="ico_list ico_list1" style="display: inline"
              >좌석우위</span
            >
            <span class="movie_title"
              ><a>{{ reservation.programName }}</a></span
            >
            <span class="movie_date">
              {{ formatDate(reservation.programStartDate) }} ~
              {{ formatDate(reservation.programEndDate) }}
            </span>
            <span class="movie_spot" title="충무아트센터 대극장">
              {{ reservation.locationName }}
            </span>
          </p>
        </div>
      </td>
      <td>
        <div class="booking_infor">
          <dl>
            <dt>예매일</dt>
            <dd>{{ formatDate(reservation.createdAt) }}</dd>
          </dl>
          <dl>
            <dt>예약번호</dt>
            <dd>
              <span class="fc_green">{{ reservation.ticketNumber }}</span>
            </dd>
          </dl>
          <dl>
            <dt>관람일</dt>
            <dd>
              {{ formatDateTime(reservation.date) }}
            </dd>
          </dl>
          <dl>
            <dt>교환요청</dt>
            <dd>{{ reservation.requestLimit }}회</dd>
          </dl>
        </div>
      </td>
      <td class="lst">
        <div class="here">
          <router-link
            :to="'/reservation/' + reservation.reservationId"
            class="btn_flexible btn_arr"
            ><span>예매 상세</span></router-link
          >
        </div>
        <div class="here" @click="openPopup">
          <a class="btn_flexible btn_arr"><span>교환 요청</span></a>
        </div>
      </td>
    </tr>
  </tbody>
</template>

<script setup>
import { defineProps } from 'vue';
import { formatDate, formatDateTime } from '@/utils/formatDate';
import { useBookingStore } from '@/stores/useBookingStore';

const props = defineProps(['reservation']);

const bookingStore = useBookingStore();

const openPopup = () => {
  bookingStore.setReservationId(props.reservation.reservationId);

  bookingStore.setData(
    props.reservation.locationId,
    props.reservation.programName,
    props.reservation.programId,
    props.reservation.timesId,
    props.reservation.sectionId
  );

  const popUrl = '/exchangeSeat';
  const popOption =
    'width=986, height=682, resizable=no, scrollbars=no, status=no;'; // 팝업창 옵션
  window.open(popUrl, '', popOption);
};
</script>

<style scoped>
.exchange_list {
  display: flex;
  justify-content: space-between;
}

.box_sorting_menu {
  position: absolute;
  top: 20px;
  right: 0;
}

.here {
  border: 2px solid #c1c1c1;
  border-radius: 5px;
  width: 100px;
  margin: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.wrap_person {
  margin-top: 40px;
  border: 1px solid #ddd;
  background: #fafafa;
  overflow: hidden;
}

img {
  border: 0 none;
}

i {
  font-style: italic;
}

.btn_line {
  border: 1px solid #ccc;
  height: 18px;
  padding: 8px 12px 8px;
}
.box_person_state {
  float: right;
  position: relative;
  padding: 40px 0;
  overflow: hidden;
}
.box_person_state li {
  float: left;
  width: 150px;
  border-right: 1px solid #eee;
}
.box_person_state li .state_num {
  font-size: 32px;
  line-height: 32px;
  text-align: center;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
}
.box_person_state li .state_num a {
  text-decoration: none;
}
.box_person_state li .state_title {
  margin-top: 11px;
  font-size: 13px;
  text-align: center;
}
.box_person_state li.lst {
  border-right: none;
}
.box_person_state li .state_num a {
  text-decoration: none;
}
.btn_line {
  border: 1px solid #ccc;
  height: 18px;
  padding: 8px 12px 8px;
}
a {
  color: #666;
  text-decoration: none;
}
.warp_ticket {
  position: relative;
  margin-top: 20px;
}
.box_answer .tit_sub_float,
.box_event .tit_sub_float,
.warp_ticket .tit_sub_float {
  font-weight: 700;
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
.ctr {
  position: absolute;
  top: 13px;
  right: 0;
}
.ctr {
  position: absolute;
  top: 18px;
  right: 0;
}
.ctr .ico_more {
  display: inline-block;
  height: 30px;
  padding-right: 12px;
  background: url(//cdnticket.melon.co.kr/resource/image/web/common/ico_more.png)
    top right no-repeat;
  font-size: 13px;
  line-height: 30px;
  color: #888;
}
.ctr .ico_more {
  display: inline-block;
  height: 30px;
  padding-right: 12px;
  background: url(//cdnticket.melon.co.kr/resource/image/web/common/ico_more.png)
    top right no-repeat;
  font-size: 13px;
  line-height: 30px;
  color: #999;
}
.box_ticket_list table.tbl_style02 {
  border-top: 1px solid #e9e9e9;
  table-layout: fixed;
}
table.tbl_style02 {
  width: 100%;
  border-top: 1px solid #e9e9e9;
}
.tbl {
  border-collapse: collapse;
  border-spacing: 0;
}
.hide {
  display: none;
}
table.tbl_style02 thead th {
  padding: 12px 0 10px;
  border-bottom: 1px solid #e9e9e9;
  font-size: 12px;
  color: #333;
  font-weight: 400;
  text-align: center;
}
.box_ticket_list table.tbl_style02 tbody td.fst {
  padding-top: 43px;
}
.box_ticket_list table.tbl_style02 tbody td {
  padding: 25px 0 24px;
  font-size: 16px;
}
table.tbl_style02 tbody td {
  border-bottom: 1px solid #e9e9e9;
  text-align: center;
  vertical-align: top;
}
.box_ticket_list table.tbl_style02 tbody td {
  padding: 25px 0 24px;
  font-size: 16px;
}
table.tbl_style02 tbody td {
  border-bottom: 1px solid #e9e9e9;
  text-align: center;
  vertical-align: top;
}
.box_ticket_list table.tbl_style02 tbody td .movie_infor .img {
  position: absolute;
  top: 0;
  left: 0;
}
.thumb_90x125,
.thumb_90x125 .frame {
  width: 90px;
  height: 125px;
}
.box_ticket_list table.tbl_style02 tbody td .movie_infor .img a {
  width: 90px;
  height: 125px;
  overflow: hidden;
  display: block;
}
.box_ticket_list table.tbl_style02 tbody td .movie_infor .img img {
  width: 90px;
}

.thumb_90x125 img,
.thumb_90x90 img {
  vertical-align: top;
}
img {
  border: 0 none;
}
.thumb_90x125 .frame {
  background: 0 0;
  width: 88px;
  height: 123px;
  z-index: 10;
  left: 0;
  border: 1px solid #000;
  opacity: 0.03;
}
.thumb_90x125,
.thumb_90x125 .frame {
  width: 90px;
  height: 125px;
}

.thumb_320x400 .frame,
.thumb_90x125 .frame {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  background: url(//cdnticket.melon.co.kr/resource/image/web/common/bg_frame.png)
    no-repeat;
}
.box_ticket_list table.tbl_style02 tbody td .movie_infor .infor_text {
  padding-left: 104px;
  font-size: 13px;
}
.box_ticket_list table.tbl_style02 tbody td .movie_infor .infor_text .ico_list {
  margin-right: 5px;
}
.ico_list1 {
  background-color: #41d26b;
}
.ico_list {
  float: left;
  overflow: hidden;
  height: 14px;
  padding: 1px 5px;
  font-size: 11px;
  line-height: 16px;
  color: #fff;
  text-align: center;
}
.box_ticket_list
  table.tbl_style02
  tbody
  td
  .movie_infor
  .infor_text
  .movie_title {
  display: block;
  width: 100%;
  height: 35px;
  padding-top: 5px;
  overflow: hidden;
}
.box_ticket_list table.tbl_style02 tbody td .movie_infor .infor_text {
  padding-left: 104px;
  font-size: 13px;
}
.box_ticket_list table.tbl_style02 tbody td .movie_infor {
  position: relative;
  height: 126px;
  padding-right: 50px;
  text-align: left;
  overflow: hidden;
}
.box_ticket_list table.tbl_style02 tbody td {
  padding: 25px 0 24px;
  font-size: 16px;
}
table.tbl_style02 tbody td {
  border-bottom: 1px solid #e9e9e9;
  text-align: center;
  vertical-align: top;
}
.box_ticket_list
  table.tbl_style02
  tbody
  td
  .movie_infor
  .infor_text
  .movie_title
  a {
  line-height: 18px;
  color: #333;
}
.box_ticket_list
  table.tbl_style02
  tbody
  td
  .movie_infor
  .infor_text
  .movie_date {
  display: block;
  margin-top: 13px;
  color: #666;
  font-size: 12px;
  letter-spacing: 0;
}
.box_ticket_list
  table.tbl_style02
  tbody
  td
  .movie_infor
  .infor_text
  .movie_spot {
  display: block;
  margin-top: 1px;
  color: #666;
  height: auto;
  max-height: 38px;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  word-wrap: break-word;
}
.box_ticket_list table.tbl_style02 tbody td {
  padding: 25px 0 24px;
  font-size: 16px;
}
table.tbl_style02 tbody td {
  border-bottom: 1px solid #e9e9e9;
  text-align: center;
  vertical-align: top;
}
.box_ticket_list table.tbl_style02 tbody td .booking_infor {
  padding-top: 19px;
  text-align: left;
  font-size: 13px;
}
.box_ticket_list table.tbl_style02 tbody td .booking_infor dt {
  display: inline-block;
  width: 63px;
  margin-bottom: 5px;
  vertical-align: top;
}
.box_ticket_list table.tbl_style02 tbody td .booking_infor dd {
  display: inline-block;
  margin-bottom: 5px;
  letter-spacing: 0;
}
.fc_green {
  color: #00b523;
}
.box_ticket_list table.tbl_style02 tbody td.lst {
  padding-top: 44px;
}
.box_ticket_list table.tbl_style02 tbody td {
  padding: 25px 0 24px;
  font-size: 16px;
}
table.tbl_style02 tbody td {
  border-bottom: 1px solid #e9e9e9;
  text-align: center;
  vertical-align: top;
}
.box_ticket_list table.tbl_style02 tbody td.lst p {
  margin-bottom: 13px;
  color: #333;
}
table .btn_flexible {
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
.box_no_list {
  height: 143px;
  border: 1px solid #eee;
  padding-top: 91px;
  overflow: hidden;
}
.box_no_list p {
  color: #333;
  font-size: 20px;
  text-align: center;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
}
.wrap_taken {
  margin-top: 24px;
}
.wrap_taken .box_event {
  position: relative;
  float: left;
  width: 492px;
}
.box_answer .tit_sub_float,
.box_event .tit_sub_float,
.warp_ticket .tit_sub_float {
  font-weight: 700;
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
.ctr {
  position: absolute;
  top: 13px;
  right: 0;
}
.ctr .ico_more {
  display: inline-block;
  height: 30px;
  padding-right: 12px;
  background: url(//cdnticket.melon.co.kr/resource/image/web/common/ico_more.png)
    top right no-repeat;
  font-size: 13px;
  line-height: 30px;
  color: #888;
}
.wrap_taken .list_evnt {
  border: 1px solid #eee;
  padding: 6px 23px 8px;
  margin-top: 4px;
  height: 218px;
}
.box_no_list2 {
  height: 143px;
  border: 1px solid #eee;
  padding-top: 91px;
  overflow: hidden;
}
.box_no_list2 p {
  color: #333;
  font-size: 20px;
  text-align: center;
  font-family: AppleSDGothicNeo-Regular, '맑은 고딕', 'Malgun Gothic';
}
.box_no_list2 .btn {
  display: block;
  margin-top: 25px;
  font-size: 12px;
  color: #666;
}
.box_no_list2 .btn .btn_line {
  border: 1px solid #ccc;
  height: 18px;
  padding: 6px 16px 6px;
}
.btn_line {
  border: 1px solid #ccc;
  height: 18px;
  padding: 8px 12px 8px;
}
.wrap_taken .box_answer {
  position: relative;
  float: right;
  width: 492px;
}
.box_answer .tit_sub_float,
.box_event .tit_sub_float,
.warp_ticket .tit_sub_float {
  font-weight: 700;
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
.wrap_taken .list_evnt {
  border: 1px solid #eee;
  padding: 6px 23px 8px;
  margin-top: 4px;
  height: 218px;
}
.wrap_taken .list_evnt li.lst {
  border-bottom: none;
}
.wrap_taken .list_evnt li {
  padding: 16px 0 16px;
  border-bottom: 1px solid #eee;
}
.wrap_taken .list_evnt li .event_title a {
  font-size: 14px;
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  max-width: 84%;
  vertical-align: top;
  padding-right: 3px;
}
.wrap_taken .list_evnt li .event_title .btn_flexible_ico1 {
  margin-left: 5px;
  vertical-align: -3px;
}
.btn_flexible_ico1 {
  background-position: left -110px;
  padding-left: 9px;
}
.btn_flexible_ico1,
.btn_flexible_ico2,
.btn_flexible_ico3 {
  display: inline-block;
  overflow: hidden;
  height: 18px;
  padding-left: 11px;
}

.btn_flexible_ico1 span {
  background-position: right -110px;
  padding-right: 8px;
  color: #00b523;
}
.btn_flexible_ico1 span,
.btn_flexible_ico2 span,
.btn_flexible_ico3 span {
  display: inline-block;
  height: 17px;
  padding: 1px 11px 0 0;
  font-size: 11px;
  line-height: 18px;
  text-align: center;
  vertical-align: top;
}
</style>
