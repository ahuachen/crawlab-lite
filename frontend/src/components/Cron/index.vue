<style lang="scss" scoped>
  #change-crontab {
    .language {
      position: absolute;
      right: 25px;
      z-index: 1;
    }

    .cron-wrapper {
      margin-bottom: 10px;
    }

    .el-tabs {
      box-shadow: none;
    }

    .tabBody {
      .el-row {
        margin: 10px 0;

        .long {
          .el-select {
            width: 350px;
          }
        }

        .el-input-number {
          width: 110px;
        }
      }
    }

    .bottom {
      width: 100%;
      text-align: center;
      margin-top: 5px;
      position: relative;

      .value {
        font-size: 18px;
        vertical-align: middle;
      }
    }
  }
</style>
<template>
  <div id="change-crontab">
    <div class="cron-wrapper">
      <label>
        {{ $t('Cron Expression') }}:
      </label>
      <el-tag type="success" size="small">
        {{ cron }}
      </el-tag>
    </div>
    <el-tabs type="border-card">
      <el-tab-pane>
        <span slot="label"><i class="el-icon-date" /> {{ text.Minutes.name }}</span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="minute.cronEvery" label="1">{{ text.Minutes.every }}</el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="minute.cronEvery" label="2">{{ text.Minutes.interval[0] }}
              <el-input-number v-model="minute.incrementIncrement" size="small" :min="0" :max="59" />
              {{ text.Minutes.interval[1] }}
              <el-input-number v-model="minute.incrementStart" size="small" :min="0" :max="59" />
              {{ text.Minutes.interval[2]||'' }}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="minute.cronEvery" class="long" label="3">{{ text.Minutes.specific }}
              <el-select v-model="minute.specificSpecific" size="small" multiple>
                <el-option v-for="val in 60" :key="val" :value="(val-1).toString()" :label="val-1" />
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="minute.cronEvery" label="4">{{ text.Minutes.cycle[0] }}
              <el-input-number v-model="minute.rangeStart" size="small" :min="0" :max="59" />
              {{ text.Minutes.cycle[1] }}
              <el-input-number v-model="minute.rangeEnd" size="small" :min="0" :max="59" />
              {{ text.Minutes.cycle[2] }}
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane>
        <span slot="label"><i class="el-icon-date" /> {{ text.Hours.name }}</span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="hour.cronEvery" label="1">{{ text.Hours.every }}</el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="hour.cronEvery" label="2">{{ text.Hours.interval[0] }}
              <el-input-number v-model="hour.incrementIncrement" size="small" :min="0" :max="23" />
              {{ text.Hours.interval[1] }}
              <el-input-number v-model="hour.incrementStart" size="small" :min="0" :max="23" />
              {{ text.Hours.interval[2] }}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="hour.cronEvery" class="long" label="3">{{ text.Hours.specific }}
              <el-select v-model="hour.specificSpecific" size="small" multiple>
                <el-option v-for="val in 24" :key="val" :value="(val-1).toString()" :label="val-1" />
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="hour.cronEvery" label="4">{{ text.Hours.cycle[0] }}
              <el-input-number v-model="hour.rangeStart" size="small" :min="0" :max="23" />
              {{ text.Hours.cycle[1] }}
              <el-input-number v-model="hour.rangeEnd" size="small" :min="0" :max="23" />
              {{ text.Hours.cycle[2] }}
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane>
        <span slot="label"><i class="el-icon-date" /> {{ text.Day.name }}</span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="day.cronEvery" label="1">{{ text.Day.every }}</el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="2">{{ text.Day.intervalDay[0] }}
              <el-input-number v-model="day.incrementIncrement" size="small" :min="1" :max="31" />
              {{ text.Day.intervalDay[1] }}
              <el-input-number v-model="day.incrementStart" size="small" :min="1" :max="31" />
              {{ text.Day.intervalDay[2] }}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" class="long" label="3">{{ text.Day.specificDay }}
              <el-select v-model="day.specificSpecific" size="small" multiple>
                <el-option v-for="val in 31" :key="val" :value="val.toString()" :label="val" />
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="4">{{ text.Day.cycle[0] }}
              <el-input-number v-model="day.rangeStart" size="small" :min="1" :max="31" />
              {{ text.Day.cycle[1] }}
              <el-input-number v-model="day.rangeEnd" size="small" :min="1" :max="31" />
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane>
        <span slot="label"><i class="el-icon-date" /> {{ text.Month.name }}</span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="month.cronEvery" label="1">{{ text.Month.every }}</el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="month.cronEvery" label="2">{{ text.Month.interval[0] }}
              <el-input-number v-model="month.incrementIncrement" size="small" :min="0" :max="12" />
              {{ text.Month.interval[1] }}
              <el-input-number v-model="month.incrementStart" size="small" :min="0" :max="12" />
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="month.cronEvery" class="long" label="3">{{ text.Month.specific }}
              <el-select v-model="month.specificSpecific" size="small" multiple>
                <el-option v-for="val in 12" :key="val" :label="val" :value="val.toString()" />
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="month.cronEvery" label="4">{{ text.Month.cycle[0] }}
              <el-input-number v-model="month.rangeStart" size="small" :min="1" :max="12" />
              {{ text.Month.cycle[1] }}
              <el-input-number v-model="month.rangeEnd" size="small" :min="1" :max="12" />
              {{ text.Month.cycle[2] }}
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane>
        <span slot="label"><i class="el-icon-date" /> {{ text.Week.name }}</span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="week.cronEvery" label="1">{{ text.Week.every }}</el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="week.cronEvery" class="long" label="3">{{ text.Week.specific }}
              <el-select v-model="week.specificSpecific" size="small" multiple>
                <el-option v-for="i in 7" :key="i" :label="text.Week.list[i - 1]" :value="i.toString()" />
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="week.cronEvery" label="4">{{ text.Week.cycle[0] }}
              <el-input-number v-model="week.rangeStart" size="small" :min="1" :max="7" />
              {{ text.Week.cycle[1] }}
              <el-input-number v-model="week.rangeEnd" size="small" :min="1" :max="7" />
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
  import Language from './language/index'

  export default {
    name: 'VueCronLinux',
    // eslint-disable-next-line vue/require-prop-types
    props: ['data', 'i18n'],
    data() {
      return {
        second: {
          cronEvery: '',
          incrementStart: '3',
          incrementIncrement: '5',
          rangeStart: '',
          rangeEnd: '',
          specificSpecific: [0]
        },
        minute: {
          cronEvery: '',
          incrementStart: '3',
          incrementIncrement: '5',
          rangeStart: '',
          rangeEnd: '',
          specificSpecific: ['0']
        },
        hour: {
          cronEvery: '',
          incrementStart: '3',
          incrementIncrement: '5',
          rangeStart: '',
          rangeEnd: '',
          specificSpecific: ['0']
        },
        day: {
          cronEvery: '',
          incrementStart: '1',
          incrementIncrement: '1',
          rangeStart: '',
          rangeEnd: '',
          specificSpecific: ['1'],
          cronLastSpecificDomDay: 1,
          cronDaysBeforeEomMinus: '',
          cronDaysNearestWeekday: ''
        },
        month: {
          cronEvery: '',
          incrementStart: '3',
          incrementIncrement: '5',
          rangeStart: '',
          rangeEnd: '',
          specificSpecific: ['1']
        },
        week: {
          cronEvery: '',
          incrementStart: '1',
          incrementIncrement: '1',
          specificSpecific: ['1'],
          cronNthDayDay: 1,
          cronNthDayNth: '1',
          rangeStart: '',
          rangeEnd: ''
        },
        output: {
          second: '',
          minute: '',
          hour: '',
          day: '',
          month: '',
          Week: '',
          year: ''
        }
      }
    },
    computed: {
      text() {
        return Language[this.i18n || 'cn']
      },
      minutesText() {
        let minutes = ''
        const cronEvery = this.minute.cronEvery
        switch (cronEvery.toString()) {
        case '1':
          minutes = '*'
          break
        case '2':
          minutes = this.minute.incrementStart + '/' + this.minute.incrementIncrement
          break
        case '3':
          this.minute.specificSpecific.map(val => {
            minutes += val + ','
          })
          minutes = minutes.slice(0, -1)
          break
        case '4':
          minutes = this.minute.rangeStart + '-' + this.minute.rangeEnd
          break
        }
        return minutes
      },
      hoursText() {
        let hours = ''
        const cronEvery = this.hour.cronEvery
        switch (cronEvery.toString()) {
        case '1':
          hours = '*'
          break
        case '2':
          hours = this.hour.incrementStart + '/' + this.hour.incrementIncrement
          break
        case '3':
          this.hour.specificSpecific.map(val => {
            hours += val + ','
          })
          hours = hours.slice(0, -1)
          break
        case '4':
          hours = this.hour.rangeStart + '-' + this.hour.rangeEnd
          break
        }
        return hours
      },
      daysText() {
        let days = ''
        const cronEvery = this.day.cronEvery
        switch (cronEvery.toString()) {
        case '1':
          days = '*'
          break
        case '2':
          days = this.day.incrementStart + '/' + this.day.incrementIncrement
          break
        case '3':
          this.day.specificSpecific.map(val => {
            days += val + ','
          })
          days = days.slice(0, -1)
          break
        case '4':
          days = this.day.rangeStart + '-' + this.day.rangeEnd
          break
        }
        return days
      },
      monthsText() {
        let months = ''
        const cronEvery = this.month.cronEvery
        switch (cronEvery.toString()) {
        case '1':
          months = '*'
          break
        case '2':
          months = this.month.incrementStart + '/' + this.month.incrementIncrement
          break
        case '3':
          this.month.specificSpecific.map(val => {
            months += val + ','
          })
          months = months.slice(0, -1)
          break
        case '4':
          months = this.month.rangeStart + '-' + this.month.rangeEnd
          break
        }
        return months
      },
      weeksText() {
        let weeks = ''
        const cronEvery = this.week.cronEvery
        switch (cronEvery.toString()) {
        case '1':
          weeks = '*'
          break
        case '3':
          this.week.specificSpecific.map(val => {
            weeks += val + ','
          })
          weeks = weeks.slice(0, -1)
          break
        case '4':
          weeks = this.week.rangeStart + '-' + this.week.rangeEnd
          break
        }
        return weeks
      },
      cron() {
        return [this.minutesText, this.hoursText, this.daysText, this.monthsText, this.weeksText]
          .filter(v => !!v)
          .join(' ')
      }
    },
    watch: {
      data() {
        this.updateCronFromData()
      },
      cron() {
        this.$emit('change', this.cron)
      }
    },
    mounted() {
      this.updateCronFromData()
    },
    methods: {
      change() {
        this.$emit('change', this.cron)
        this.close()
      },
      close() {
        this.$emit('close')
      },
      submit() {
        if (!this.validate()) {
          this.$message.error(this.$t('Cron expression is invalid'))
          return false
        }
        this.$emit('submit', this.cron)
        return true
      },
      validate() {
        if (!this.minutesText) return false
        if (!this.hoursText) return false
        if (!this.daysText) return false
        if (!this.monthsText) return false
        if (!this.weeksText) return false
        return true
      },
      updateCronItem(key, value) {
        if (value === undefined) {
          this[key].cronEvery = '0'
          return
        }
        if (value.match(/^\*$/)) {
          this[key].cronEvery = '1'
        } else if (value.match(/\//)) {
          this[key].cronEvery = '2'
          this[key].incrementStart = value.split('/')[0]
          this[key].incrementIncrement = value.split('/')[1]
        } else if (value.match(/,|^\d+$/)) {
          this[key].cronEvery = '3'
          this[key].specificSpecific = value.split(',')
        } else if (value.match(/-/)) {
          this[key].cronEvery = '4'
          this[key].rangeStart = value.split('-')[0]
          this[key].rangeEnd = value.split('-')[1]
        } else {
          this[key].cronEvery = '0'
        }
      },
      updateCronFromData() {
        if (!this.data) {
          return
        }
        const arr = this.data.split(' ')
        const minute = arr[0]
        const hour = arr[1]
        const day = arr[2]
        const month = arr[3]
        const week = arr[4]

        this.updateCronItem('minute', minute)
        this.updateCronItem('hour', hour)
        this.updateCronItem('day', day)
        this.updateCronItem('month', month)
        this.updateCronItem('week', week)
      }
    }
  }</script>
