<template>
<div>
  <AdminTaskTable :tasks='taskList' />
</div>
</template>

<style scoped>
table {
  margin: 0 auto;
}
.space {
  margin: 20px auto;
}
</style>

<script>
import { mapGetters, mapActions } from 'vuex'
import { sortBy, filter } from 'lodash'
import Auth from 'Auth'
import moment from 'moment'
import AdminTask from '@/components/AdminTask'
import DatePicker from 'vuejs-datepicker'
import AdminTaskTable from '@/components/AdminTaskTable'

export default {
  name: 'TaskListRoute',
  components: {
    AdminTask,
    DatePicker,
    AdminTaskTable
  },
  computed: {
    taskList () {
      return sortBy(filter(this.sortedTasksWithDate(this.startDate, this.endDate), task => task.created_by === Auth.getUser().id), ['completed', 'user_id', 'start_time'])
    },
    ...mapGetters(['sortedTasksWithDate', 'users'])
  },
  data () {
    return {
      startDate: moment().day(1).hour(8).minute(0).format('YYYY-MM-DD'),
      endDate: moment().day(5).hour(18).minute(0).format('YYYY-MM-DD'),
      selectedUser: ''
    }
  },
  methods: {
    chooseDate2 (date) {
      this.startDate = moment(date).day(1).format('YYYY-MM-DD')
      this.endDate = moment(date).day(5).hour(18).format('YYYY-MM-DD')
      // console.log(moment(date).day(1).hour(18).toDate())
    },
    ...mapActions(['getAllTasks', 'getAllUsers', 'getAllClients'])
  }
}
</script>
