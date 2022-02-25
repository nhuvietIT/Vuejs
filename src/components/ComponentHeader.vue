<template>
  <div class="comp-headers">
    <div class="comp-header">
      <h1>TODOS</h1>
      <input
        type="text"
        class="add-todo"
        placeholder="What needs to be done ?"
        v-model="todoName"
        @keyup.enter="AddTodoEvent"
      />
    </div>
    <component-tab>
      <ul class="tab-header">
        <li
          v-for="(item, index) in tabHeader"
          v-bind:key="index"
          @click="HandleClickTab(item.active)"
          v-bind:class="{ activecolor: activeTab === item.active }"
        >
          {{ item.text }}
        </li>
      </ul>

      <div class="list-todo" v-if="activeTab === 'all'">
        <h5 class="title-todos">Name Todos</h5>
        <span class="checkall-todos">Check All</span>
        <input type="checkbox" @click="CheckAll" v-model="isCheckAll" />
        <b-list-group v-for="item in todos" :key="item.id">
          <b-list-group-item class="justify-content-between align-items-center">
            <input
              type="checkbox"
              v-bind:value="item"
              v-model="todoCheckAll"
              @change="updateCheckAll"
            />
            <b-icon
              class="icondelete"
              @click="HandleDeleteTodo(item.id)"
              icon="trash"
              variant="danger"
            />
            <button
              v-show="checkUpdate === true && idCheck === item.id"
              class="btn-save"
              @click="HandleUpdateTodo(item)"
            >
              Save
              <b-icon
                v-show="checkUpdate === true && idCheck === item.id"
                icon="pencil"
                variant="success"
              />
            </button>

            <input
              v-show="checkUpdate === true && idCheck === item.id"
              class="textupdateEdit"
              type="text"
              v-bind:value="item.name"
              @change="HandleEditTodoName"
              @keyup.enter="HandleUpdateTodo(item)"
            />
            <input
              v-show="checkUpdate === true && idCheck !== item.id"
              class="textupdate"
              type="text"
              v-bind:value="item.name"
              readonly
            />

            <b-icon
              v-show="checkUpdate === false"
              class="icon-fix"
              @click="HandleCheckUpdateTodo(item.id)"
              icon="pencil"
              variant="secondary"
            />
            <button
              v-bind:class="{
                btnActive: 'active' !== item.active
              }"
              class="btn-save"
              @click="HandleUpdateActiveTodo(item)"
            >
              <div class="btnCompleted" v-show="item.active === 'completed'">
                Completed
              </div>
              <div v-show="item.active !== 'completed'">
                Actite
              </div>
            </button>
            <input
              v-show="checkUpdate === false"
              class="textupdate"
              type="text"
              v-bind:value="item.name"
              readonly
            />
          </b-list-group-item>
        </b-list-group>
        <button
          v-show="activeTab === 'all'"
          class="all-delete"
          type="button"
          @click="clearAll()"
        >
          CLEAR ALL
          <!-- <b-icon icon="trash" variant="danger" /> -->
        </button>
      </div>
      <div class="list-todo" v-else-if="activeTab === 'active'">
        <h5 class="title-todos">Name Todos</h5>
        <span class="checkall-todos">Check All</span>
        <input
          type="checkbox"
          @click="checkAllActive"
          v-model="isCheckAllActive"
        />

        <b-list-group v-for="item in filterTodoActive" :key="item.id">
          <b-list-group-item class="justify-content-between align-items-center">
            <input
              type="checkbox"
              v-bind:value="item"
              v-model="todoCheckActive"
              @change="updateCheckAllActive"
            />
            <b-icon
              class="icondelete"
              @click="HandleDeleteTodo(item.id)"
              icon="trash"
              variant="danger"
            />
            <button
              v-show="checkUpdate === true && idCheck === item.id"
              class="btn-save"
              @click="HandleUpdateTodo(item)"
            >
              Save
              <b-icon
                v-show="checkUpdate === true && idCheck === item.id"
                icon="pencil"
                variant="success"
              />
            </button>

            <input
              v-show="checkUpdate === true && idCheck === item.id"
              class="textupdateEdit"
              type="text"
              v-bind:value="item.name"
              @change="HandleEditTodoName"
              @keyup.enter="HandleUpdateTodo(item)"
            />
            <input
              v-show="checkUpdate === true && idCheck !== item.id"
              class="textupdate"
              type="text"
              v-bind:value="item.name"
              readonly
            />

            <b-icon
              v-show="checkUpdate === false"
              class="icon-fix"
              @click="HandleCheckUpdateTodo(item.id)"
              icon="pencil"
              variant="secondary"
            />
            <button
              v-bind:class="{
                btnActive: 'active' !== item.active
              }"
              class="btn-save"
              @click="HandleUpdateActiveTodo(item)"
            >
              <div class="btnCompleted" v-show="item.active === 'completed'">
                Completed
              </div>
              <div v-show="item.active !== 'completed'">
                Actite
              </div>
            </button>
            <input
              v-show="checkUpdate === false"
              class="textupdate"
              type="text"
              v-bind:value="item.name"
              readonly
            />
          </b-list-group-item>
        </b-list-group>
        <input
          v-show="activeTab === 'active'"
          class="completed"
          type="button"
          @click="updateCompleted()"
          value="COMPLETED"
        />
      </div>
      <div class="list-todo" v-else>
        <h5 class="title-todos">Name Todos</h5>
        <span class="checkall-todos">Check All</span>
        <input
          type="checkbox"
          @click="checkAllCompleted"
          v-model="isCheckAllCompleted"
        />

        <b-list-group v-for="item in filterTodocompleted" :key="item.id">
          <b-list-group-item class="justify-content-between align-items-center">
            <input
              type="checkbox"
              v-bind:value="item"
              v-model="todoCheckCompleted"
              @change="updateCheckAllCompleted"
            />
            <b-icon
              class="icondelete"
              @click="HandleDeleteTodo(item.id)"
              icon="trash"
              variant="danger"
            />
            <button
              v-show="checkUpdate === true && idCheck === item.id"
              class="btn-save"
              @click="HandleUpdateTodo(item)"
            >
              Save
              <b-icon
                v-show="checkUpdate === true && idCheck === item.id"
                icon="pencil"
                variant="success"
              />
            </button>

            <input
              v-show="checkUpdate === true && idCheck === item.id"
              class="textupdateEdit"
              type="text"
              v-bind:value="item.name"
              @change="HandleEditTodoName"
              @keyup.enter="HandleUpdateTodo(item)"
            />
            <input
              v-show="checkUpdate === true && idCheck !== item.id"
              class="textupdate"
              type="text"
              v-bind:value="item.name"
              readonly
            />

            <b-icon
              v-show="checkUpdate === false"
              class="icon-fix"
              @click="HandleCheckUpdateTodo(item.id)"
              icon="pencil"
              variant="secondary"
            />
            <button
              v-bind:class="{
                btnActive: 'active' !== item.active
              }"
              class="btn-save"
              @click="HandleUpdateActiveTodo(item)"
            >
              <div class="btnCompleted" v-show="item.active === 'completed'">
                Completed
              </div>
              <div v-show="item.active !== 'completed'">
                Actite
              </div>
            </button>
            <input
              v-show="checkUpdate === false"
              class="textupdate"
              type="text"
              v-bind:value="item.name"
              readonly
            />
          </b-list-group-item>
        </b-list-group>
        <input
          v-show="activeTab === 'completed'"
          class="clear-completed"
          type="button"
          @click="ClearCompleted()"
          value="CLEAR COMPLETED"
        />
      </div>
    </component-tab>
  </div>
</template>
<script>
import ComponentTab from "./ComponentTab.vue";
import "../assets/css/todos.css";
import {
  BIcon,
  BIconTrash,
  BListGroup,
  BListGroupItem,
  BFormCheckbox,
  BFormCheckboxGroup,
  BIconPencil,
  BIconCheck
} from "bootstrap-vue";
export default {
  components: {
    ComponentTab,
    BIcon,
    BIconTrash,
    BListGroup,
    BListGroupItem,
    BFormCheckbox,
    BFormCheckboxGroup,
    BIconPencil,
    BIconCheck
  },
  name: "comp-header",
  props: {
    todos: Array
  },
  data() {
    return {
      tabHeader: [
        { text: "All", active: "all" },
        { text: "Actite", active: "active" },
        { text: "Completed", active: "completed" }
      ],
      activeTab: "all",
      todoName: "",
      todoNameUpdate: "",
      isCheckAll: false,
      isCheckAllActive: false,
      isCheckAllCompleted: false,
      todoCheckAll: [],
      todoCheckActive: [],
      todoCheckCompleted: [],
      selectedlang: "",
      checkUpdate: false,
      idCheck: ""
    };
  },
  methods: {
    HandleClickTab(active) {
      this.activeTab = active;
      this.isCheckAll = false;
      this.isCheckAllActive = false;
      this.isCheckAllCompleted = false;
      this.todoCheckAll = [];
      this.todoCheckActive = [];
      this.todoCheckCompleted = [];
    },
    AddTodoEvent() {
      this.$emit("AddTodoEvent", this.todoName);
      this.todoName = "";
    },
    CheckAll() {
      this.isCheckAll = !this.isCheckAll;
      this.todoCheckAll = [];
      if (this.isCheckAll) {
        // Check all
        for (let key in this.todos) {
          this.todoCheckAll.push(this.todos[key]);
        }
      }
    },
    updateCheckAll() {
      if (this.todoCheckAll.length == this.todos.length) {
        this.isCheckAll = true;
      } else {
        this.isCheckAll = false;
      }
    },
    checkAllActive() {
      this.isCheckAllActive = !this.isCheckAllActive;
      this.todoCheckActive = [];
      if (this.isCheckAllActive) {
        // Check all
        for (let key in this.filterTodoActive) {
          this.todoCheckActive.push(this.filterTodoActive[key]);
        }
      }
    },
    updateCheckAllActive() {
      if (this.todoCheckActive.length == this.filterTodoActive.length) {
        this.isCheckAllActive = true;
      } else {
        this.isCheckAllActive = false;
      }
    },
    checkAllCompleted() {
      this.isCheckAllCompleted = !this.isCheckAllCompleted;
      this.todoCheckCompleted = [];
      if (this.isCheckAllCompleted) {
        // Check all
        for (let key in this.filterTodocompleted) {
          this.todoCheckCompleted.push(this.filterTodocompleted[key]);
        }
      }
    },
    updateCheckAllCompleted() {
      if (this.todoCheckCompleted.length == this.filterTodocompleted.length) {
        this.isCheckAllCompleted = true;
      } else {
        this.isCheckAllCompleted = false;
      }
    },
    updateCompleted() {
      const dataList = this.todoCheckActive.map(({ id, name, active }) => ({
        id: id,
        name: name,
        active: "completed"
      }));
      if (dataList.length > 0) this.$emit("updateTodo", dataList);
      this.isCheckAllActive = false;
    },
    ClearCompleted() {
      const dataList = this.todoCheckCompleted.map(({ id, name, active }) => ({
        id: id
      }));
      if (dataList.length > 0) this.$emit("deleteTodoEvent", dataList);
      this.isCheckAllCompleted = false;
      this.todoCheckCompleted = [];
    },
    HandleDeleteTodo(id) {
      const dataList = {
        id: id
      };
      this.$emit("deleteTodoEvent", dataList);
    },
    clearAll() {
      const dataList = this.todoCheckAll.map(({ id, name, active }) => ({
        id: id
      }));
      if (dataList.length > 0) this.$emit("deleteTodoEvent", dataList);
      this.isCheckAll = false;
      this.todoCheckAll = [];
    },
    HandleCheckUpdateTodo(id) {
      this.idCheck = id;
      if (!this.checkUpdate) {
        this.checkUpdate = true;
      } else {
        this.checkUpdate = false;
      }
    },
    HandleUpdateTodo(item) {
      this.idCheck = item.id;
      const data = {
        id: item.id,
        name: this.todoNameUpdate,
        active: item.active
      };
      if (this.todoNameUpdate !== "") this.$emit("updateTodo", data);
      if (!this.checkUpdate) {
        this.checkUpdate = true;
      } else {
        this.checkUpdate = false;
      }
      this.todoNameUpdate = "";
    },
    HandleEditTodoName(e) {
      this.todoNameUpdate = e.target.value;
    },
    HandleUpdateActiveTodo(item) {
      if (item.active === "active") {
        const data = {
          id: item.id,
          name: item.name,
          active: "all"
        };
        this.$emit("updateTodo", data);
      } else if (item.active === "all") {
        const data = {
          id: item.id,
          name: item.name,
          active: "active"
        };
        this.$emit("updateTodo", data);
      } else {
        const data = {
          id: item.id,
          name: item.name,
          active: item.active
        };
        this.$emit("updateTodo", data);
      }
      this.todoCheckAll = [];
      this.isCheckAll = false
    }
  },
  computed: {
    filterTodoActive() {
      return this.todos.filter(t => t.active === "active");
    },
    filterTodocompleted() {
      return this.todos.filter(t => t.active === "completed");
    }
  }
};
</script>
