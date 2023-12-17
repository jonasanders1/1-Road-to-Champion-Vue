<template>
  <div class="container">

    <div class="tags">
      <div class="form">
        <input type="text" placeholder="Tag here.." v-model="newTag" @keyup.enter="addTag" @keyup.space="addTag" />
      </div>

      <div class="tags-container">
        <div class="pill" v-for="tag in tags" :key="tag" @click="deleteTag(tag)">
          {{ tag }}
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      newTag: "",
      tags: []
    }
  },
  methods: {
    addTag() {
      if (this.newTag.trim()) {
        if (this.tags.includes(this.newTag.toLowerCase().trim())) {
          alert("Can't add tags with the same name");
          return;
        } else {
          this.tags.push(this.newTag.toLowerCase().trim());
          this.newTag = "";
        }
      }
    },
    deleteTag(tag) {
      const index = this.tags.indexOf(tag)
      if (index > -1) {
        this.tags.splice(index, 1);
      }
    }
  },
  // watch every time the tags array changes, aka useEffect(() => {})
  watch: {
    names: {
      handler(newNames) {
        console.log(newNames)
      },
      deep: true
    }
  }
}
</script>

<style scoped>
.container {
  background-color: rgb(247, 247, 247);
  padding: 10px;
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family: sans-serif;
}

.form {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;

}

.container>* {
  box-sizing: border-box;
}

.tags-container {
  display: flex;
  gap: 10px;
  margin-top: 20px;
  flex-direction: row;
  flex-wrap: wrap;
  min-height: 100px;
  width: 350px;
  justify-content: center;
}

button {
  padding: 10px;
  height: 40px;
  width: 100px;
  font-size: 1.2rem;
  border: none;
  background-color: rgb(71, 132, 189);
  border-radius: 8px;
  color: white;
}

input {
  padding: 10px;
  border: 3px solid rgb(71, 132, 189);
  background-color: transparent;
  border-radius: 8px;
  outline: none;
}

input:focus {
  border: 3px solid rgb(75, 71, 189);
}

.pill {
  background-color: rgb(71, 132, 189);
  color: white;
  border-radius: 50px;
  padding: 10px 20px;
  height: fit-content;
  text-transform: capitalize;
}
</style>