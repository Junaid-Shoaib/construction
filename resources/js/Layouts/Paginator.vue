<template>
  <nav
    v-if="balances !== undefined"
    class="flex items-center justify-between"
    role="navigation"
  >
    <div class="flex justify-between flex-1 sm:hidden">
      <span
        v-if="onFirstPage"
        class="
          relative
          inline-flex
          items-center
          px-4
          py-2
          text-sm
          font-medium
          text-gray-500
          bg-gray-100
          border border-gray-300
          cursor-default
          leading-5
          rounded-md
        "
      >
        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
          <path
            clip-rule="evenodd"
            d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
            fill-rule="evenodd"
          />
        </svg>
      </span>
      <Link
        v-else
        :href="previousPageUrl"
        class="
          relative
          inline-flex
          items-center
          px-4
          py-2
          text-sm
          font-medium
          text-gray-700
          bg-white
          border border-gray-300
          leading-5
          rounded-md
          hover:text-gray-500
          focus:outline-none focus:ring
          ring-gray-300
          focus:border-blue-300
          active:bg-gray-100 active:text-gray-700
          transition
          ease-in-out
          duration-150
        "
      >
        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
          <path
            clip-rule="evenodd"
            d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
            fill-rule="evenodd"
          />
        </svg>
      </Link>

      <Link
        v-if="hasMorePages"
        :href="nextPageUrl"
        class="
          relative
          inline-flex
          items-center
          px-4
          py-2
          ml-3
          text-sm
          font-medium
          text-gray-700
          bg-white
          border border-gray-300
          leading-5
          rounded-md
          hover:text-gray-500
          focus:outline-none focus:ring
          ring-gray-300
          focus:border-blue-300
          active:bg-gray-100 active:text-gray-700
          transition
          ease-in-out
          duration-150
        "
      >
        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
          <path
            clip-rule="evenodd"
            d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
            fill-rule="evenodd"
          />
        </svg>
      </Link>
      <span
        v-else
        class="
          relative
          inline-flex
          items-center
          px-4
          py-2
          ml-3
          text-sm
          font-medium
          text-gray-500
          bg-gray-100
          border border-gray-300
          cursor-default
          leading-5
          rounded-md
        "
      >
        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
          <path
            clip-rule="evenodd"
            d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
            fill-rule="evenodd"
          />
        </svg>
      </span>
    </div>

    <div
      class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between px-16"
    >
      <div>
        <p class="text-sm text-gray-700 leading-5">
          Showing
          <span class="font-medium" v-text="firstItem"></span>
          to
          <span class="font-medium" v-text="lastItem"></span>
          of
          <span class="font-medium" v-text="total"></span>
          results
        </p>
      </div>

      <div class="float-none px-16">
        <span class="relative z-0 inline-flex shadow-sm rounded-md">
          <span
            v-if="onFirstPage"
            aria-disabled="true"
            aria-hidden="true"
            class="
              relative
              inline-flex
              items-center
              px-2
              py-2
              text-sm
              font-medium
              text-gray-500
              bg-gray-100
              border border-gray-300
              cursor-default
              rounded-l-md
              leading-5
            "
          >
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
              <path
                clip-rule="evenodd"
                d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                fill-rule="evenodd"
              />
            </svg>
          </span>
          <Link
            v-else
            :href="previousPageUrl"
            class="
              relative
              inline-flex
              items-center
              px-2
              py-2
              text-sm
              font-medium
              text-gray-500
              bg-white
              border border-gray-300
              rounded-l-md
              leading-5
              hover:text-gray-400
              focus:z-10 focus:outline-none focus:ring
              ring-gray-300
              focus:border-blue-300
              active:bg-gray-100 active:text-gray-500
              transition
              ease-in-out
              duration-150
            "
            rel="prev"
          >
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
              <path
                clip-rule="evenodd"
                d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                fill-rule="evenodd"
              />
            </svg>
          </Link>

          <div v-for="link in balances.links" :key="link.id">
            <div v-if="link.label != 'Previous'">
              <Link
                v-if="
                  !isFirstOrLastOrDots(index, balances.links.length, link.label)
                "
                :class="{ 'bg-blue-200': link.active === true }"
                :href="link.url"
                class="
                  relative
                  inline-flex
                  items-center
                  px-4
                  py-2
                  -ml-px
                  text-sm
                  font-medium
                  text-gray-700
                  bg-white
                  border border-gray-300
                  leading-5
                  hover:text-gray-500
                  focus:z-10 focus:outline-none focus:ring
                  ring-gray-300
                  focus:border-blue-300
                  active:bg-gray-100 active:text-gray-700
                  transition
                  ease-in-out
                  duration-150
                "
              >
                {{ link.label }}
              </Link>
              <span
                v-else-if="link.label === '...'"
                aria-disabled="true"
                class="
                  relative
                  inline-flex
                  items-center
                  px-4
                  py-2
                  -ml-px
                  text-sm
                  font-medium
                  text-gray-700
                  bg-white
                  border border-gray-300
                  cursor-default
                  leading-5
                "
              >
                {{ link.label }}
              </span>
            </div>
          </div>
          <Link
            v-if="hasMorePages"
            :href="nextPageUrl"
            class="
              relative
              inline-flex
              items-center
              px-2
              py-2
              -ml-px
              text-sm
              font-medium
              text-gray-500
              bg-white
              border border-gray-300
              rounded-r-md
              leading-5
              hover:text-gray-400
              focus:z-10 focus:outline-none focus:ring
              ring-gray-300
              focus:border-blue-300
              active:bg-gray-100 active:text-gray-500
              transition
              ease-in-out
              duration-150
            "
          >
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
              <path
                clip-rule="evenodd"
                d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                fill-rule="evenodd"
              />
            </svg>
          </Link>
          <span
            v-else
            aria-disabled="true"
            aria-hidden="true"
            class="
              relative
              inline-flex
              items-center
              px-2
              py-2
              -ml-px
              text-sm
              font-medium
              text-gray-500
              bg-gray-100
              border border-gray-300
              cursor-default
              rounded-r-md
              leading-5
            "
          >
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
              <path
                clip-rule="evenodd"
                d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                fill-rule="evenodd"
              />
            </svg>
          </span>
        </span>
      </div>
    </div>
  </nav>
</template>

<script>
import TermsOfService from "@/Pages/TermsOfService";
import { Head, Link } from "@inertiajs/inertia-vue3";

export default {
  name: "balances",
  components: {
    TermsOfService,
    Head,
    Link,
  },
  props: {
    balances: {
      current_page: Number,
      data: Array,
      first_page_url: String,
      from: Number,
      last_page: Number,
      last_page_url: String,
      links: Array,
      next_page_url: String,
      path: String,
      per_page: Number,
      prev_page_url: String,
      to: Number,
      total: Number,
    },
  },
  methods: {
    isFirstOrLastOrDots(index, links_length, label) {
      return index === 0 || index === links_length - 1 || label.includes("...");
    },
  },
  computed: {
    onFirstPage() {
      return this.balances.current_page === 1;
    },
    hasMorePages() {
      return this.balances.current_page < this.balances.last_page;
    },
    nextPageUrl() {
      return this.balances.next_page_url;
    },
    previousPageUrl() {
      return this.balances.prev_page_url;
    },
    firstItem() {
      return this.balances.from;
    },
    lastItem() {
      return this.balances.to;
    },
    total() {
      return this.balances.total;
    },
  },
};
</script>