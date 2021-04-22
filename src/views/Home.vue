<template>
  <v-container
    fluid
  >
    <v-row
      class="d-flex justify-center pb-3"
    >
      <v-col
        xl="6"
        lg="6"
        md="8"
        class=""
      >
        <h1
          class="text-center deep-purple lighten-1 pa-10 white--text rounded"
        >
          Vuetify Todo
        </h1>
      </v-col>
    </v-row>

    <v-row
      class="d-flex justify-center"
    >
      <v-col
        xl="6"
        lg="6"
        md="8"
      >
        <div class="pa-3 deep-purple lighten-1 rounded">
          <v-dialog
            v-model="dialog.show"
            :value="true"
            persistent
            max-width="290"
          >
            <v-card>
              <v-card-title class="headline">
                Delete Task?
              </v-card-title>
              <v-card-text>Are you sure you wanna delete the task?</v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn
                  text
                  @click="dialog.show = false"
                >
                  NO
                </v-btn>
                <v-btn
                  color="red darken-1"
                  text
                  @click="deleteBtn()"
                >
                  YES
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-text-field
            v-model="newTodoTitle"
            hide-details
            clearable
            class="white pa-3"
            outlined
            label="Add Task"
            append-icon="mdi-plus-circle blue--text"
            @click:append="addTodo"
            @keyup.enter="addTodo"
          />
          <v-list
            class="pa-0"
            flat
          >
            <div
              v-for="(product, idx) in products"
              :key="idx"
              :class="{'deep-purple lighten-4' : product.done}"
              @click="product.done = !product.done"
            >
              <v-list-item
                class="pb-0"
              >
                <template v-slot:default>
                  <v-list-item-action>
                    <v-checkbox
                      :color="'deep-purple'"
                      :input-value="product.done"
                    />
                  </v-list-item-action>

                  <v-list-item-content

                    :class="{'text-decoration-line-through' : product.done,
                             'deep-purple--text darken-4' : product.done}"
                  >
                    <v-list-item-title>
                      {{ product.title }}
                    </v-list-item-title>
                  </v-list-item-content>
                  <v-list-item-action>
                    <v-btn
                      icon
                      @click.stop="dialog = {show: true, indexToDelete: idx}"
                    >
                      <v-icon color="red lighten-1">
                        mdi-delete
                      </v-icon>
                    </v-btn>
                  </v-list-item-action>
                </template>
              </v-list-item>
              <v-divider />
            </div>
          </v-list>
        </div>
      </v-col>
    </v-row>

    <v-snackbar
      v-model="snackbar.show"
      :timeout="snackbar.timeout"
    >
      {{ snackbar.text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="snackbar.show = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      dialog: {
        show: false,
        indexToDelete: -1
      },
      snackbar: {
        show: false,
        text: 'Do not forget to add some task',
        timeout: 3000
      },
      newTodoTitle: '',
      products: [
        {
          title: 'strawberry',
          done: false
        },
        {
          title: 'peach',
          done: false
        },
        {
          title: 'apple',
          done: false
        }
      ]
    }
  },
  watch: {
    products (newValue, oldValue) {
      if (newValue.length === 0) {
        this.snackbar.show = true
      } else {
        this.snackbar.show = false
      }
    }
  },
  methods: {
    deleteBtn () {
      this.products.splice(this.dialog.indexToDelete, 1)
      this.dialog.show = false
      this.dialog.indexToDelete = -1
    },
    addTodo () {
      const newTodo =
        {
          title: this.newTodoTitle,
          done: false
        }
      this.products.push(newTodo)
      this.newTodoTitle = ''
    }
  }
}
</script>
