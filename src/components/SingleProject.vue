<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="toggleDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'Edit Project', params: { id: project.id } }">
          <span class="material-icons"> edit </span>
        </router-link>
        <span class="material-icons" @click="deleteProject"> delete </span>
        <span class="material-icons tick" @click="toggleComplete"> done </span>
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showDetails: false,
      uri: `http://localhost:3000/projects/${this.project.id}`,
    };
  },
  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails;
    },
    deleteProject() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((error) => console.error(error));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => {
          this.$emit("complete", this.project.id);
        })
        .catch((error) => console.error(error));
    },
  },
  props: ["project"],
};
</script>

<style>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
  transition-duration: 0.3s;
}
.material-icons:hover {
  color: #777;
  transition-duration: 0.3s;
}
.project.complete {
  border-left: 4px solid #00ce89;
  transition-duration: 0.3s;
}
.project.complete .tick {
  color: #00ce89;
}
</style>