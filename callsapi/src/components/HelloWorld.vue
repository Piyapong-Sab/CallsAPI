<template>
  <div class="hello">
    {{result}}
    <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-container fluid>
    <v-layout row>
      <v-flex ma-2 md12>
        <v-layout>
          <v-text-field v-model="text1" prepend-icon="mdi-account" :rules="nameRules"
      label="Name" required></v-text-field>
          <v-text-field v-model="text2" prepend-icon=" " :rules="lastRules"
      label="lastname" required></v-text-field>
          <v-text-field v-model="text3" prepend-icon=" " :rules="fristRules"
      label="fristname" required ></v-text-field>
          <v-text-field v-model="text4" prepend-icon=" " :rules="ageRules"
      label="Age" required type="number" counter maxlength="10" suffix="ปี"  ></v-text-field>
          <v-text-field v-model="text5" prepend-icon=" " :rules="telphoneRules"
      label="telphone" required mask="##-####-####"></v-text-field>
            <v-text-field v-model="text6" prepend-icon=" " :rules="passwordRules"
      label="password" type="password" ></v-text-field>
          </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
  <v-btn @click="validate()" >
    submit
  </v-btn>
  </v-form>
  <v-data-table
      :headers="headers"
      :items="result"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
          <td class="text-xs-left">{{ props.item.name }}</td>
          <td class="text-xs-left">{{ props.item.lastname }}</td>
          <td class="text-xs-left">{{ props.item.fristname }}</td>
          <td class="text-xs-left">{{ props.item.age }}</td>
          <td class="text-xs-left">{{ props.item.telphone }}</td>
          <td class="text-xs-left">{{ props.item.password }}</td>
          <td>
            <v-btn icon @click="edit(props.item)">
              <v-icon color="primary" >mdi-pencil</v-icon>
            </v-btn>
            <v-btn icon @click="deleteItem(props.item)">
              <v-icon color="primary" >mdi-delete</v-icon>
            </v-btn>
          </td>
      </template>
    </v-data-table>
    <v-dialog
        v-model="dialog"
        width="500"
      >
      <v-card>
          <v-card-title
            class="headline grey lighten-2"
            primary-title
          >
          Edit
          </v-card-title>
          <v-card-text>
        <v-container fluid>
    <v-layout row>
      <v-flex ma-2 md12>
          <v-text-field v-model="dataEdit.name" prepend-icon="mdi-account" :rules="nameRules"
      label="Name" required></v-text-field>
          <v-text-field v-model="dataEdit.lastname" prepend-icon=" " :rules="lastRules"
      label="lastname" required></v-text-field>
          <v-text-field v-model="dataEdit.fristname" prepend-icon=" " :rules="fristRules"
      label="fristname" required ></v-text-field>
          <v-text-field v-model="dataEdit.age" prepend-icon=" " :rules="ageRules"
      label="Age" required type="number" counter maxlength="10" suffix="ปี"  ></v-text-field>
          <v-text-field v-model="dataEdit.telphone" prepend-icon=" " :rules="telphoneRules"
      label="telphone" required mask="##-####-####"></v-text-field>
            <v-text-field v-model="dataEdit.password" prepend-icon=" " :rules="passwordRules"
      label="password" type="password" ></v-text-field>
      </v-flex>
    </v-layout>
  </v-container>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" flat @click="close()" >cancel</v-btn>
            <v-btn color="primary" flat @click="save()">save</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: [],
      text1: '',
      text2: '',
      text3: '',
      text4: '',
      text5: '',
      text6: '',
      editedIndex: -1,
      dialog: false,
      valid: true,
      dataEdit: [],
      data: [],
      nameRules: [
        v => (v.length > 0) || 'กรุณากรอกชื่อ',
      ],
      lastRules: [
        v => (v.length > 0) || 'กรุณากรอกนามสกุล',
      ],
      fristRules: [
        v => (v.length > 0) || 'กรุณากรอกชื่อเล่น',
      ],
      ageRules: [
        v => (v.length > 0) || 'กรุณากรอกอายุ',
      ],
      telphoneRules: [
        v => (v.length > 0) || 'กรุณากรอกเบอร์โทรศัพท์',
      ],
      passwordRules: [
        v => (v.length > 0) || 'กรุณากรอกรหัสผ่าน',
      ],
      headers: [
        {
          text: 'Name',
          align: 'left',
          value: 'name',
        },
        { text: 'Lastname', value: 'lastname' },
        { text: 'Fristname', value: 'fristname' },
        { text: 'Age', value: 'age' },
        { text: 'Telphone', value: 'age' },
        { text: 'Password', value: 'password' },
        { text: 'Action', align: 'center', sortable: false },
      ],
    };
  },
  methods: {
    submit() {
      const data = {
        name: this.text1,
        lastname: this.text2,
        fristname: this.text3,
        age: this.text4,
        telphone: this.text5,
        password: this.text6,
      };
      this.result.push(data);
      console.log(this.result);
      this.reset();
    },
    validate() {
      if (this.$refs.form.validate()) {
        this.submit();
      } else {
        alert('กรุณากรอกข้อมูลให้ครบ');
      }
    },
    reset() {
      this.text1 = '';
      this.text2 = '';
      this.text3 = '';
      this.text4 = '';
      this.text5 = '';
      this.text6 = '';
    },
    edit(item) {
      this.dialog = true;
      this.editedIndex = this.result.indexOf(item);
      this.dataEdit = Object.assign({}, item);
    },
    deleteItem(item) {
      const index = this.result.indexOf(item);
      // eslint-disable-next-line no-restricted-globals
      confirm('คุณแน่ใจใช่ไหมที่จะลบ?') && this.result.splice(index, 1);
    },
    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.result);
        this.editedIndex = -1;
      }, 300);
    },
    save() {
      Object.assign(this.result[this.editedIndex], this.dataEdit);
      this.close();
    },
  },
};
</script>


<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

function newFunction() {
  this.text1;
}
