<template>
  <div id="calendar">
    <div id="header">2021/09</div>
    <div id="days">
      <div class="day">SUN</div>
      <div class="day">MON</div>
      <div class="day">TUE</div>
      <div class="day">WED</div>
      <div class="day">TUR</div>
      <div class="day">FRI</div>
      <div class="day">SAT</div>
    </div>
    <div id="dates">
      <div
        class="date-block"
        v-for="(date, index) in dates"
        :key="index"
        @dblclick="newPanel(index)"
      >
        <div class="date">{{ date+1 }}</div>
        <div class="events">
          <div class="event"  v-for="(event,index) in events" :key="index" @dblclick.stop="updatePanel(event)">
            <template v-if="date == eventId">
              <div class="title">{{events[index].title}}</div>
              <div class="from">{{events[index].start_time}}</div>
            </template>

          </div>
        </div>
      </div>
      <div class="date-block empty"></div>
      <div class="date-block empty"></div>
      <div class="date-block empty"></div>
      <div class="date-block empty"></div>
      <div class="date-block empty"></div>
    </div>
  </div>

  <div
    id="info-panel"
    v-bind:class="{ open: opened, new: isNew, update: updated }"
  >
    <div class="close" @click="close">x</div>

    <form>
      <div class="title">
        <label for="">Event</label>
        <br />
        <input type="text" name="title" ref="title" v-model="oneEvent.title">
      </div>
      <div class="time-picker">
        <div class="selected-date">
          <span class="month">9</span>
          /
          <span class="date">{{ eventId + 1 }}</span>
          <input type="hidden" name="month" />
          <input type="hidden" name="date" />
        </div>
      </div>
      <div class="from">
        <label for="from">From</label>
        <br />
        <input
          id="from"
          type="time"
          name="start_time"
          v-model.number="oneEvent.start_time"
          >
      </div>
      <div class="to">
        <label for="to">To</label>
        <br />
        <input
          id="to"
          type="time"
          name="end_time"
          v-model.number="oneEvent.end_time"
          >
      </div>
      <div class="description">
        <label for="description">Description</label>
        <br />
        <textarea
          name="description"
          id="description"
          v-model="oneEvent.description"
        ></textarea>
      </div>
    </form>

    <div class="buttons clearfix">
      <button class="create" @click="create">Create</button>
      <button class="update" @click="update">Update</button>
      <button class="cancel" @click="close">Cancel</button>
      <button class="delete" @click="deleteE">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {},
  data() {
    return {
      dates: [0,1,2,3,4,5,6,7,8,9,10
      ,11,12,13,14,15,16,17,18,19,20,
      21,22,23,24,25,26,27,28,29],
      opened: false,
      isNew: false,
      updated: false,
      eventId: null,
      events: [],
      oneEvent:{
          title: null,
          start_time: null,
          end_time: null,
          description: null,
      },
      cloneEvent:null,
    };
  },
  methods: {
    newPanel(index) {
      this.opened = true;
      this.isNew = true;
      this.eventId = index;
      this.updated = false;
      this.oneEvent= {
          title: null,
          start_time: null,
          end_time: null,
          description: null,
      };
      this.$refs.title.focus();
    },
    updatePanel() {
      this.opened = true;
      this.updated = true;
      this.isNew = false;
      this.$refs.title.focus();
    },
    close() {
      this.opened = false;
    },
    create() {

      this.cloneEvent = Object.assign({},this.oneEvent);
      this.events.push(this.cloneEvent);

      this.close();
    },
    update(){

    },
    deleteE(){
      this.events.pop()
      this.close();
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#calendar {
  margin: 0 auto;
  width: 90%;
}

#header {
  font-size: 2.4rem;
  font-weight: bold;
}

#days,
#dates {
  display: flex;
  flex-wrap: wrap;
}

#dates {
  border-right: 1px solid #ccc;
  border-bottom: 1px solid #ccc;
}

.day,
.date-block {
  flex: 0 0 14.28%;
}

.date-block {
  border-top: 1px solid #ccc;
  border-left: 1px solid #ccc;
  height: 15vh;
  padding: 4px;
}

.date-block.empty {
  background: #eee;
}

#dates .event {
  display: flex;
  margin-bottom: 2px;
  border-radius: 12px;
  text-align: center;
  background: orange;
  color: white;
}

#dates .title,
#dates .from {
  flex: 0 0 50%;
}

/* info-panel */
#info-panel {
  display: none;
  position: absolute;
  top: 25%;
  right: 37%;
  width: 25vw;
  background: white;
  border: 1px solid #ccc;
}

#info-panel.open {
  display: block;
}

#info-panel .close {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

#info-panel label {
  font-size: 0.8rem;
  color: #aaa;
}

#info-panel .title,
#info-panel .time-picker {
  border-bottom: 1px solid #ccc;
}

#info-panel .title,
#info-panel .time-picker,
#info-panel .description {
  padding: 10px;
}

#info-panel .selected-date {
  font-size: 1.4rem;
  text-align: center;
}

#description {
  width: 100%;
}

#info-panel button {
  display: none;
  border: none;
  padding: 10px;
  cursor: pointer;
  background: #aaa;
  color: white;
}

#info-panel.new button.create,
#info-panel.new button.cancel {
  display: block;
  float: left;
  width: 50%;
}

#info-panel.update button.update,
#info-panel.update button.cancel,
#info-panel.update button.delete {
  display: block;
  float: left;
  width: 50%;
}

#info-panel.update button.delete {
  width: 100%;
  background: #c21717;
}

#info-panel.new button.create,
#info-panel.update button.update {
  background: #74be00;
}
</style>
