<!-- 仮想 DOM を構成する DOM を定義 -->
<template>
  <div id="app">
    <table>
      <tbody>
        <tr>
          <th>ID</th>
          <th>name</th>
          <th>department</th>
          <th>gender</th>
        </tr>
        <tr v-for="e in employees" :key="e.id">
          <!-- v-XXX(v-for)配列の繰り返し, :YYY(:key) は Vue.js のディレクティブ(参考) -->
          <!-- :YYY は省略記法で v-bind:YYY ディレクティブの場合に使える書き方 -->
          <!-- :key を v-for と組み合わせることにより、繰り返し作成される各 DOM 要素に一意の ID をつけている -->
          <td><router-link :to="{name: 'EmployeeDetailPage', params: {id: e.id}}" >{{ e.id }}</router-link></td>
          <!-- <router-link> によりルータを使って遷移できるリンクを作成 -->
          <td>{{ e.name }}</td>
          <td>{{ e.department }}</td>
          <td>{{ e.gender }}</td>
          <td>
            <button @click="deleteTarget = e.id; showModal = true">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    <modal v-if="showModal" @cancel="showModal = false" @ok="deleteEmployee(); showModal = false;">
      <!-- v-ifは値がfalseの場合にはコンポーネントを非表示にし、trueの場合には表示するためのディレクティブ -->
      <div slot="body">Are you sure?</div>
      <!-- <modal></modal> に含まれる DOM を Modal コンポーネントの template で <slot> として参照できるようにする機能で、<div slot="body"></div> のように記述すると <slot name="body"> のように名前付きで呼び出すことが出来る -->
    </modal>
  </div>
</template>

<!-- script は仮想 DOM に関連する JavaScript を記述 -->
<script>
// Vue.js で API を利用するための方法として、公式ページでも紹介されている axios を使う
// script ではまず axios を import しています。これで script 内で axios を使える
import axios from 'axios';
import Modal from 'Modal.vue';

export default {
  components:{
    Modal
  },
  // data が仮想 DOM が保持するデータ
  data: function () {
    // return されるハッシュがそのデータを表し、これにより key である message が Vue.js で利用できる
    return {
      // template に書かれた {{ employees }} はこのデータを指している
      // 初期値として空配列を設定しておき、AJAX を使ってモデル一覧が取得出来たら上書きする
      employees: [],
      showModal: false,
      deleteTarget: -1,
      errors: ''
    }
  },
  // mounted は Vue.js におけるライフサイクルにおいて、仮想 DOM が DOM に置き換わるタイミングを指す。つまりまずは employees が空配列の状態で template を使って生成された DOM が表示される
  // (table のヘッダ行のみが存在して、内容が空)
  mounted () {
    this.updateEmployees();
  },
  methods: {
    deleteEmployee: function() {
      if (this.deleteTarget <= 0){
        console.warn('deleteTarget should be grater than zero.');
        return;
      }
      axios
      .delete(`api/v1/employees/${this.deleteTarget}`)
      .then(response => {
        this.deleteTarget = -1;
        this.updateEmployees();
      })
      .catch( error => {
        console.error(error);
        if (error.response.data && error.response.data.errors) {
          this.errors = error.response.data.errors;
        }
      });
    },
    updateEmployees: function() {
      // タイミングで API にアクセスしてモデルの取得を試る
      axios
        .get('/api/v1/employees.json')
        // そして正常に応答が返って来た場合に employees に受け取ったデータを格納する。
        // this は Vue コンポーネントのインスタンスを指す。this.employees により data で定義したデータを読み書き出来る
        .then(response => (this.employees = response.data))
    }
  }
}
</script>


<!-- style は仮想 DOM に適用するスタイルを css で定義 -->

<style scoped>
p {
  font-size: 2em;
  text-align: center;
}
</style>