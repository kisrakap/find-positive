<template>
  <v-container>
    <v-card class="justify-start rounded elevation-9 w-50 h-100 mx-auto">
      <v-tabs
        v-model="tab"
        color="deep-purple-accent-4"
        align-tabs="center"
        class="d-flex justify-center mx-auto"
      >
        <v-tab :value="1" :disabled="tab == 2 || tab == 3">Tab 1</v-tab>
        <v-tab :value="2" :disabled="tab === 1">Tab 2</v-tab>
        <v-tab :value="3" :disabled="tab === 1">Tab 3</v-tab>
      </v-tabs>
      <v-window v-model="tab" align-items="center" class="mx-auto pa-8">
        <v-window-item v-for="n in 3" :key="n" :value="n">
          <v-container
            class="border d-flex justify-center rounded-lg elevation-8"
          >
            <v-row v-if="tab === 1">
              <v-col cols="12">
                <h2>Tab 1 Content</h2>
                <v-form ref="form">
                  <v-text-field
                    v-model="inputValue"
                    min="1"
                    max="10000"
                    label="Enter a number"
                    :rules="NumberRules"
                    type="number"
                  ></v-text-field>
                </v-form>
                <v-btn color="success" @click="moveToTab(2)" v-show="inputValue"
                  >Next</v-btn
                >
              </v-col>
            </v-row>
            <v-row v-if="tab === 2">
              <v-col cols="12" class="text-center">
                <!-- Center the entire content -->
                <h2 class="mx-auto">Tab 2 Content</h2>
                <v-btn color="success" @click="generateArray"
                  >Generate Numbers</v-btn
                >
                <v-container>
                  <v-card-title>
                    <span class="headline ma-4">List of Numbers</span>
                  </v-card-title>
                  <v-data-table
                    class="border pa-2 ma-4 text-center"
                    :headers="headers"
                    :items="randomNumbers"
                    item-value="number"
                    v-if="tab === 2"
                    :disable-pagination="randomNumbers.length === 0"
                  >
                  </v-data-table>
                  <v-btn
                    class="ma-4"
                    color="success"
                    @click="moveToTab(3)"
                    v-if="tab === 2 && randomNumbers.length > 0"
                    >Next</v-btn
                  >
                </v-container>
              </v-col>
            </v-row>

            <v-row v-if="tab === 3">
              <v-col cols="12" class="d-flex justify-center">
                <div class="w-50 justify-center">
                  <h2>Tab 3 Content</h2>
                  <v-btn
                    @click.prevent="calculateResult"
                    class="ma-5"
                    color="primary"
                    >Find B</v-btn
                  >
                  <div>
                    <h3 v-if="tab === 3">
                      Bilangan B: {{ smallestPositiveNumber }}
                    </h3>
                  </div>
                  <div class="d-flex ma-9 ml-auto">
                    <v-btn
                      color="cyan"
                      @click="reset"
                      v-if="tab === 3 && smallestPositiveNumber"
                      >Reset</v-btn
                    >
                  </div>
                </div>
              </v-col>
            </v-row>
          </v-container>
        </v-window-item>
      </v-window>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      tab: null,
      inputValue: null,
      randomNumbers: [],
      smallestPositiveNumber: null,
      NumberRules: [
        (v) => !!v || "Value is required",
        (v) =>
          /^[1-9]\d*$/.test(v) ||
          "Value must be a positive integer and larger than 0",
        (v) => (v >= 1 && v <= 10000) || "Value must be between 1 and 10000",
      ],
      headers: [{ text: "Number", value: "number" }],
    };
  },
  methods: {
    moveToTab(tab) {
      if (tab === 1) {
        this.$refs.form.validate();
      }
      this.tab = tab;
    },
    generateArray() {
      if (this.inputValue) {
        const uniqueNumbers = new Set();

        while (uniqueNumbers.size < this.inputValue) {
          const randomNumber = Math.floor(Math.random() * 20000) - 10000;
          uniqueNumbers.add(randomNumber);
        }

        this.randomNumbers = Array.from(uniqueNumbers).map((number, index) => ({
          number,
          id: index,
        }));
      }
    },
    findSmallestPositiveNotInArray() {
      const positiveIntegers = Array.from(
        new Set(this.randomNumbers.filter((num) => num.number > 0))
      );

      if (positiveIntegers.length === 0) {
        return 1;
      }
      const smallestPositiveInArray = Math.min(
        ...positiveIntegers.map((item) => item.number)
      );

      if (smallestPositiveInArray === 1) {
        let result = 2;
        while (positiveIntegers.includes(result)) {
          result++;
        }
        return result;
      }
      return smallestPositiveInArray - 1;
    },
    calculateResult() {
      this.smallestPositiveNumber = this.findSmallestPositiveNotInArray();
      // console.log(this.smallestPositiveNumber);
    },
    reset() {
      this.tab = 1;
      this.inputValue = null;
      this.randomNumbers = [];
      this.smallestPositiveNumber = null;
    },
  },
};
</script>

<style></style>
