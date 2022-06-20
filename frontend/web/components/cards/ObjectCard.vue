<template>
  <v-card
    v-if="orientation == 'horizontal'"
    class="object__card--horizontal rounded-lg elevation-1"
  >
    <v-container>
      <v-row>
        <v-col class="pb-0" style="max-width: 270px">
          <v-img
            min-width="224px"
            max-width="260px"
            lazy-src="/2.jpg"
            src="/2.jpg"
            class="rounded-lg"
          ></v-img>
        </v-col>
        <v-col class="pb-0 d-flex align-end flex-column">
          <v-card-subtitle
            class="align-self-start text--secondary text-caption  pa-0"
            >{{ value.type.title }}</v-card-subtitle
          >
          <v-card-title
            class="align-self-start text-h6 pt-0 pb-5 pl-0 text-break"
            style="line-height: 1.3rem"
          >
            {{ value.title }}
          </v-card-title>
          <v-card-text class="mt-1 grow">
            <v-row>
              <span class="mr-4">
                <v-icon color="primary"></v-icon>
                {{ maxGuests | word_case(['гость', 'гостя', 'гостя']) }}
              </span>
<!--              <span class="mr-4">
                <v-icon color="accent"></v-icon>
                {{ rooms | word_case(['комната', 'комнаты', 'комнат']) }}
              </span>-->
              <span class="mr-4">
                <v-icon color="accent"></v-icon>
                {{ rooms | word_case(['кровать', 'кровати', 'кроватей']) }}
              </span>
              <span class="mr-4">
                <v-icon color="accent"></v-icon>
                {{ rooms | word_case(['спальня', 'спальни', 'спален']) }}
              </span>
              <span class="mr-4">
                <v-icon></v-icon>

                {{ square }} м<sup>2</sup>
              </span>
            </v-row>
            <v-row class="mt-5">
              <v-chip
                v-for="item in value.options"
                v-show="item.primary"
                :key="item.id"
                outlined
                small
                :value="item.id"
                class="ma-1 ml-0"
              >
                <v-icon small left>{{ item.icon }}</v-icon>
                {{ item.title }}
              </v-chip>
            </v-row>
          </v-card-text>
          <v-spacer></v-spacer>
          <v-card-actions
            class="object__card--horizontal__card-actions pb-0 mt-auto d-flex w-100"
          >
            <v-flex class="d-inline-flex align-center">
              <v-rating
                :value="value.rating.value"
                color="amber"
                dense
                half-increments
                readonly
                size="14"
              ></v-rating>
              <div class="grey--text text-caption">
                {{ value.rating.value }} ({{ value.reviews.count }} отзывов)
              </div>
            </v-flex>
            <v-flex class="text--darken-2 text--secondary text-h6 text-right">
              <span>{{ value.price }}</span>
            </v-flex>
          </v-card-actions>
        </v-col>
      </v-row>
    </v-container>
  </v-card>

  <v-card
    v-else-if="orientation == 'vertical'"
    style="width: 350px"
    class="object__card--vertical mx-auto"
    :class="{ 'object__card--map': map }"
  >
    <v-container>
      <v-img
        height="250"
        contain
        lazy-src="https://cdn.vuetifyjs.com/images/cards/cooking.png"
        src="https://cdn.vuetifyjs.com/images/cards/cooking.png"
        :class="{ 'object__card__image--map': map }"
      >
      </v-img>
      <v-card-subtitle
        class="text--secondary text-caption  pt-1 pb-0"
        >{{ value.type.title }}</v-card-subtitle
      >
      <v-card-title
        class="text-h6 pt-0 pb-5 text-break"
        style="line-height: 1.3rem"
      >
        {{ value.title }}
      </v-card-title>
      <v-card-text>
        <v-row align="center" style="width: 90%">
          <v-rating
            :value="value.rating.value"
            color="red"
            dense
            half-increments
            readonly
            size="14"
          ></v-rating>
          <div class="grey--text text-caption">
            {{ value.rating.value }} ({{ value.reviews.count }})
          </div>
          <div
            class="text--darken-2 text--secondary text-h6 text-right ml-auto"
          >
            <span>{{ value.price  }}</span>
          </div>
        </v-row>
      </v-card-text>
      <v-divider class="mx-4"></v-divider>
    </v-container>
  </v-card>
</template>

<script>
import collect from 'collect.js'

export default {
  name: 'ObjectCard',
  props: {
    orientation: {
      // horizontal || vertical
      type: String,
      default: 'horizontal',
    },
    value: {
      type: Object,
      default: () => (
        {
        id: '1',
        title: 'Уютная однокомнатная квартира с видом на море',


          price: 'от 4500 ₽ / ночь',
        type: {
          id: 1,
          name: 'flat',
          title: 'Квартира',
        },

        params: [
          { id: '1', name: 'square', title: 'Площадь', value: '40' },
          { id: '2', name: 'rooms', title: 'Количество комнат', value: '2' },

          {
            id: '3',
            name: 'max_guests',
            title: 'Максимальное количество гостей',
            value: '4',
          },
        ],
        options: [
          {
            id: '1',
            name: 'wifi',
            title: 'WI FI',
            order: 0,
            primary: true,
            icon: 'mdi-wifi',
          },
          {
            id: '9',
            name: 'pets',
            title: 'Pets friendly',
            order: 1,
            primary: true,
            icon: 'mdi-waves',
          },
          {
            id: '10',
            name: 'stir',
            title: 'Бесплатная отмена',
            order: 1,
            primary: true,
            icon: 'mdi-check',
          },

        ],
        rating: {
          value: 4.75,
          detail: {},
        },
        reviews: {
          count: 8,
          list: [],
        },
      }),
    },
    map: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    /**
     * Collection of the value
     * @return {Collection<unknown>}
     */
    val() {
      return collect(this.value)
    },
    params() {
      return collect(this.val.get('params')).keyBy('name')
    },
    square() {
      return this.params.get('square').value
    },
    rooms() {
      return this.params.get('rooms').value
    },
    maxGuests() {
      return this.params.get('max_guests').value
    },
  },
  methods: {},
}
</script>

<style>
.object__card--map .v-image .v-image__image--preload {
  filter: none !important;
}
.object__card__image--map .v-image .v-image__image--preload {
  filter: none !important;
}
.object__card--horizontal__card-actions {
  width: 100%;
}
</style>
