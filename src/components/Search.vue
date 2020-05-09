<template>
  <form class="form-inline float-left search-form">
    <b-input-group size="sm" class="mx-sm-1 mb-2">
      <template v-if="!isEmptyObject(searchBySelectOptions)" v-slot:prepend>
        <b-dropdown :text="searchBySelectLabel" size="sm" variant="outline-secondary" class="search-by-dropdown">
          <b-dropdown-item v-for="(label, value) in searchBySelectOptions" :key="value" v-on:click="handleSearchByClick(value)">{{ label }}</b-dropdown-item>
        </b-dropdown>
        <input type="hidden" :name="searchByName" v-model="searchBySelectValue" />
      </template>

      <b-form-input :name="searchName" v-model="searchInputValue" placeholder="Search phrase..." :class="searchInputClasses"></b-form-input>

      <template v-slot:append>
        <b-button v-if="searchValue" type="reset" variant="outline-secondary" v-on:click="handleReset" class="search-reset-btn">
          <i class="mr-1 mdi mdi-close"></i>
        </b-button>
        <b-button type="submit" variant="outline-secondary" v-on:click="handleSubmit">
          <i class="mr-1 mdi mdi-magnify"></i>
        </b-button>
      </template>
    </b-input-group>
  </form>
</template>

<script>
  import Vue from 'vue';

  export default {
    name: 'ui-widget-search',
    props: {
      searchName: {
        type: String,
        default: 'search'
      },
      searchValue: {
        type: String
      },
      searchByName: {
        type: String,
        default: 'searchBy',
      },
      searchByValue: {
        type: String
      },
      searchByValueOptions: {
        type: String
      }
    },
    data() {
      return {
        searchInputValue: this.searchValue,
        searchInputClasses: {},
        searchBySelectValue: this.searchByValue,
        searchBySelectLabel: '',
        searchBySelectOptions: JSON.parse(this.searchByValueOptions)
      }
    },
    mounted() {
      this.searchBySelectValue = this.getSearchBySelectValue();
      this.searchBySelectLabel = this.getSearchBySelectLabel();
    },
    methods: {
      handleSubmit(e) {
        if (!this.searchInputValue) {
          return this.handleReset(e);
        }
      },
      handleReset(e) {
        let url = new URL(window.location.href);
        let params = new URLSearchParams(url.search.slice(1));

        url.searchParams.delete('search');
        url.searchParams.delete('searchBy');
        window.location.href = url;

        e.preventDefault();
        return false;
      },
      handleSearchByClick(value) {
        this.searchBySelectValue = value;
        this.searchBySelectLabel = this.getSearchBySelectLabel();
      },
      getSearchBySelectValue() {
        return this.searchBySelectValue || Object.keys(this.searchBySelectOptions)[0];
      },
      getSearchBySelectLabel() {
        return this.searchBySelectOptions[this.searchBySelectValue];
      },
      isEmptyObject(someObject) {
        return Object.keys(someObject).length === 0;
      }
    }
  }
</script>

<style lang="scss">
  .search-form {
    .search-by-dropdown .btn-outline-secondary {
      border: 1px solid #ced4da;
    }

    .input-group-append {
      position: relative;

      .btn + .btn {
        margin-left: 0;
      }

      .search-reset-btn {
        border: none;
        position: absolute;
        right: 100%;
        top: 0;
        bottom: 0;
      }
    }
  }
</style>
