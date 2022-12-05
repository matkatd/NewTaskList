<template>
  <div id="task-list">
    <v-list-item-group v-for="i in Tasks" :key="i._id">
      <!-- TODO: Add a v-for attribute on this tag -->
      <!-- Remember to use the :key attribute to define a key for each task! -->
      <v-list-item>
        <template>
          <v-list-item-action v-on:click="clickCheckBox(i)">
            <!-- TODO: Add a click event handler attribute here that updates an task -->
            <v-btn icon>
              <!-- TODO: Add a v-if and v-else check here based on the current task's Done property -->
              <v-icon v-if="i.Done">check_box</v-icon>
              <v-icon v-else>check_box_outline_blank</v-icon>
            </v-btn>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>
              {{ i.Text }}
              <!-- TODO: Show the value of the task's Text property here -->
            </v-list-item-title>
            <v-list-item-subtitle>
              {{ i.Date }}
              <!-- TODO: Show the value of the task's Date property here -->
            </v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action v-on:click="clickDelete(i)">
            <!-- TODO: Add a click event handler attribute here that deletes this task -->
            <v-btn icon>
              <v-icon color="red">delete</v-icon>
            </v-btn>
          </v-list-item-action>
        </template>
      </v-list-item>
    </v-list-item-group>
  </div>
</template>

<script>
import { cookies } from "@/util";
export default {
  name: "TaskList",
  props: {
    Tasks: {
      Type: Array,
      Default: () => [],
    },
    // TODO: Figure out what you need to bring in from props, then and then add those variables here
    // Make sure to include these props on the component's tag when using it in another file
    // HINT: For the tasks prop, set the default value to:
    // default: () => []
  },
  methods: {
    clickCheckBox(task) {
      this.$emit("checked", task);
    },
    clickDelete(task) {
      this.$emit("deleted", task);
    },
  },
};
</script>
