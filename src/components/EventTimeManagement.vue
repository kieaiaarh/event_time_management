<template>
  <div>
    <div class="field">
      <div class="control">
        <label for="admins_event_form_event_room_open_times" class="label"
          >イベント開催日</label
        >
        <br />
        {{ _eventStartTimeUnix }}
        <br />
        {{ _distractMinutesUnix }}
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
          :value="openRoomTime"
          hide-disabled-hours
          disabled
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
          :disabled="timeUsingZoom != ''"
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
      defalut: "17:40",
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
    timeUsingZoom: {
      type: Number,
      default: 30,
    },
  },
  data() {
    return {
      eventDay: this.initialEventDay,
      roomOpenTime: this.initialRoomOpenTime,
      clientStartTime: this.initialClientStartTime,
      clientEndTime: "",
      eventStartTime: this.initialEventStartTime,
      eventEndTime: this.initialEventEndTime,
    };
  },
  created() {
    if (this.timeUsingZoom > 0) {
      const timeUsingZoomUnix = this.timeUsingZoom * (1000 * 60);
      const d = new Date(`${this.eventDay} ${this.eventEndTime}`);
      const e = d.getTime() + timeUsingZoomUnix;
      const t = new Date(e);
      const [h, m] = [t.getHours(), t.getMinutes()];
      this.clientEndTime = `${this.zeroPadding(h)}:${this.zeroPadding(m)}`;
    }
  },
  methods: {
    zeroPadding(t) {
      return t.toString().padStart(2, "0");
    },
  },
  computed: {
    //基本はCの30分前、BがCの20分以上前になる場合はBの10分前に設定される)
    openRoomTime() {
      const _this = this;
      const unixOpenRoomTime =
        _this._eventStartTimeUnix - _this._distractMinutesUnix;
      const date = new Date(unixOpenRoomTime);
      const [h, m] = [date.getHours(), date.getMinutes()];
      return `${this.zeroPadding(h)}:${this.zeroPadding(m)}`;
    },
    _distractMinutesUnix() {
      const _this = this;
      if (!_this.clientStartTime) return 1800000;
      const distractMinutes =
        _this._eventStartTimeUnix - _this._clientStartTimeUnix;
      return distractMinutes >= 1200000 ? 600000 : 1800000;
    },
    _eventStartTimeUnix() {
      const _this = this;
      if (!_this.eventStartTime) return 0;
      const d = new Date(`${_this.eventDay} ${_this.eventStartTime}`);
      return d.getTime();
    },
    _clientStartTimeUnix() {
      const _this = this;
      if (!_this.clientStartTime) return 0;
      if (!_this.clientStartTime["kk"]) return 0;
      if (!_this.clientStartTime["mm"]) return 0;
      const d = new Date(
        `${_this.eventDay} ${_this.clientStartTime.kk}:${_this.clientStartTime.mm}`
      );
      return d.getTime();
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