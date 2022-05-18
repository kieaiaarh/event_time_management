<template>
  <div>
    <div class="field">
      <div class="control">
        <label for="admins_event_form_event_room_open_times" class="label"
          >イベント開催日</label
        >
        <datepicker
          :name="nameAttribute"
          :class="{ 'w-one-quarter': datepickerStyleClass }"
          hour-label="時"
          minute-label="分"
          :input-class="'input is-info '"
          :format="'yyyy年MM月dd日'"
          v-model="eventDay"
        ></datepicker>
      </div>
    </div>

    <div class="field is-grouped">
      <div class="control">
        <label for="admins_event_form_room_open_time" class="label"
          >ルームオープン時刻</label
        >
        <vue-timepicker
          placeholder="例）13:00"
          input-class="input is-info"
          hour-label="時"
          :hour-range="Array.from(Array(14).keys(), (x) => x + 8)"
          minute-label="分"
          minute-interval="5"
          format="kk:mm"
          v-model="roomOpenTime"
          hide-disabled-hours
        ></vue-timepicker>
      </div>

      <div class="control">
        <label for="admins_event_form_client_start_time" class="label"
          >クライアント開始時刻</label
        >
        <vue-timepicker
          placeholder="例）13:00"
          input-class="input is-info"
          hour-label="時"
          :hour-range="Array.from(Array(14).keys(), (x) => x + 8)"
          minute-label="分"
          minute-interval="5"
          format="kk:mm"
          v-model="clientStartTime"
          hide-disabled-hours
        ></vue-timepicker>
      </div>

      <div class="control">
        <label for="admins_event_form_event_start_time" class="label"
          >イベント開始時刻</label
        >
        <vue-timepicker
          input-class="input is-info"
          format="kk:mm"
          hour-label="時"
          :hour-range="Array.from(Array(14).keys(), (x) => x + 8)"
          minute-label="分"
          minute-interval="5"
          placeholder="例）13:00"
          v-model="eventStartTime"
          hide-disabled-hours
        ></vue-timepicker>
      </div>

      <div class="control">
        <label for="admins_event_form_event_end_time" class="label"
          >イベント終了時刻</label
        >
        <vue-timepicker
          input-class="input is-info"
          format="kk:mm"
          placeholder="例）13:00"
          :hour-range="Array.from(Array(16).keys(), (x) => x + 8)"
          hour-label="時"
          minute-label="分"
          minute-interval="5"
          v-model="eventEndTime"
          hide-disabled-hours
        ></vue-timepicker>
      </div>

      <div v-if="useZoomAfter" class="control">
        <label for="admins_event_form_client_end_time" class="label"
          >クライアント終了時刻（懇親会など含む）</label
        >
        <vue-timepicker
          input-class="input is-info"
          format="kk:mm"
          placeholder="例）13:00"
          :hour-range="Array.from(Array(18).keys(), (x) => x + 8)"
          hour-label="時"
          minute-label="分"
          minute-interval="5"
          hide-disabled-hours
          v-model="clientEndTime"
          diabled
        ></vue-timepicker>
      </div>
    </div>
  </div>
</template>

<script>
import Datepicker from "vuejs-datepicker";
import VueTimepicker from "vue2-timepicker/src/vue-timepicker.vue";

export default {
  components: {
    Datepicker,
    VueTimepicker,
  },
  props: {
    datepickerStyleClass: {
      type: Boolean,
      default: true,
    },
    nameAttribute: {
      type: String,
      default: "admins_event_form[event_room_open_time]",
    },
    initialEventDay: {
      type: String,
      required: false,
      default: "2022-05-18",
    },
    initialRoomOpenTime: {
      type: String,
      required: false,
    },
    initialClientStartTime: {
      type: String,
      required: false,
    },
    initialClientEndTime: {
      type: String,
      required: false,
    },
    initialEventStartTime: {
      type: String,
      required: false,
      default: "18:00",
    },
    initialEventEndTime: {
      type: String,
      required: false,
      default: "19:30",
    },
    useZoomAfter: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      eventDay: this.initialEventDay,
      roomOpenTime: this.initialRoomOpenTime,
      clientStartTime: this.initialClientStartTime,
      //clientEndTime: this.initialClientEndTime,
      eventStartTime: this.initialEventStartTime,
      eventEndTime: this.initialEventEndTime,
    };
  },
  computed: {
    useableClientEndTime() {
      return useZoomAfter;
    },
  },
};
</script>

<style lang="scss">
input.display-time {
  border: 1px solid #3e8ed0 !important;
  width: 10em;
  height: 2.6em;
  padding: 1.2em 0.5em !important;
  font-size: 1em;
}
.w-one-quarter {
  width: 20%;
}
</style>