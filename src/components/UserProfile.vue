<template>
  <div class="user-profile">
    <div class="user-profile_user-panel">
      <h1 class="use-profile_username">@{{ user.username }}</h1>
      <div class="user-profile_admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile_follower-count">
        <strong>Followers: </strong>{{ followers }}
      </div>
      <form
        class="user-profile_create-twoot"
        @submit.prevent="createNewTwoot"
        :class="{ '--exceeded': newTwootCharacterCount > 180 }"
      >
        <label for="newTwoot"
          ><strong>New Twoot</strong> ({{ newTwootCharacterCount }}/180)</label
        >
        <textarea id="newTwoot" rows="4" v-model="state.newTwootContent" />
        <div class="user-profile_create-twoot-type">
          <label for="newTwootType"><strong>Type: </strong></label>
          <select id="newTwootType" v-model="state.selectedTwootType">
            <option
              :value="option.value"
              v-for="(option, index) in state.twootTypes"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
          <button class="button">Twoot!</button>
        </div>
      </form>
    </div>
    <div class="user-profile_twoots-wraper">
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";
import { reactive, computed } from "vue";

export default {
  name: "UserProfile",
  components: { TwootItem },
  setup() {
    const state = reactive({
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Twoot" },
      ],
    });
    const newTwootCharacterCount = computed(() => state.newTwootContent.length);

    function createNewTwoot() {
      if (state.newTwootContent && state.selectedTwootType !== "draft") {
        state.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
        state.newTwootContent = "";
      }
    }

    return {
      state,
      newTwootCharacterCount,
      createNewTwoot
    };
  },

  /*
  data() {
    return {
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Twoot" },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "DavidDong",
        firstName: "David",
        lastName: "Dong",
        email: "daviddong201612@gmail.com",
        isAdmin: true,
        twoots: [
          { id: 1, content: "Twotter is Amazing!" },
          { id: 2, content: "I love you Miley!" },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    },
  },
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      console.log(`Favouriteed Tweet #${id}`);
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
        this.newTwootContent = "";
      }
    },
  },
  mounted() {
    this.followUser();
  },
  */
};
</script>

<style  scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
}
.user-profile_user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px sold #dfe3e8;
}
.user-profile_admin-badge {
  background: darkgreen;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  padding: 0 10px;
  font-weight: bold;
}

h1 {
  margin: 0;
}
.user-profile_create-twoot {
  border-top: 1px solid #dfe3e8;
  padding-top: 20px;
  display: flex;
  flex-direction: column;
}
.--exceeded {
  color: red;
  border-color: red;
}
.button {
  background: darkgreen;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  padding: 0 20px;
  font-weight: bold;
  font-size: 20px;
}
</style>