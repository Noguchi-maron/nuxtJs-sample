<template>
  <section class="container">
    <h1>Todoリスト</h1>
    <div class="addArea">
      <input type="text" name="addName" id="addName" v-model="content" placeholder="タスクを入力してください">
      <button id="addButton" class="button button--green" @click="insert">追加</button>
    </div>
    <div class="Filter">
      <button class="button button--gray" :class="{'is-active':(!find_flg)}" @click="flag_reset">全て</button>
      <button class="button button--gray" :class="{'is-active':find_flg && (find_state == '作業前')}" @click="find('作業前')">作業前</button>
      <button class="button button--gray" :class="{'is-active':find_flg && (find_state == '作業中')}" @click="find('作業中')" >作業中</button>
      <button class="button button--gray" :class="{'is-active':find_flg && (find_state == '完了')}" @click="find('完了')">完了</button>
      <button class="button done-delete"  @click="doneDelete">完了したタスクを削除</button>
    </div>
    <table class="Lists">
      <thead>
        <tr>
          <th>タスク</th>
          <th>登録日時</th>
          <th>状態</th>
          <th> </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in display_todos" :key="index">
          <td>{{ item.content }}</td>
          <td>{{ item.created }}</td>
          <td><button class="button" :class="{
            'button--yet': item.state == '作業前',
            'button--progress': item.state =='作業中',
            'button--done': item.state =='完了'
          }" @click="changeState(item)">{{ item.state }}</button></td>
          <td><button class="button button-delete" @click="remove(item)">削除</button></td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
import {mapState} from 'vuex'
export default {
  data () {
    return {
      content: '',
      find_state: '',
      find_flg: false
    }
  },
  computed: {
    ...mapState(['todos']),
    display_todos () {
      if(this.find_flg) {
        let arr = [];
        let data = this.todos;
        data.forEach(element => {
          if(element.state == this.find_state) {
            arr.push(element);
          }
        });
        return arr;
      }else {
        return this.todos;
      }
    }
  },
  methods: {
    insert () {
      if (this.content != '') {
        this.$store.commit('insert', {content: this.content});
        this.content = ''
      }
    },
    remove (todo) {
      this.$store.commit('remove', todo)
    },
    changeState (todo) {
      this.$store.commit('changeState', todo)
    },
    find (findState){
    this.find_state = findState;
    this.find_flg = true;
    },
    flag_reset (){
      this.find_flg = false;
    },
    doneDelete() {
      this.$store.commit('doneDelete')
    }
  }
}
</script>


