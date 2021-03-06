<template>
  <DataTable
    :columns="columns"
    :rows="sortedPeople"
    :sort="sort"
    class="max-w-3xl mx-auto"
    @sort="updateSort"
  >
    <template #column-name="{ value, row }">
      <div class="flex items-center">
        <img :src="row.avatar_url" class="flex-shrink-0 h-12 w-12 rounded-full" />
        <div class="ml-4">
          <div class="font-semibold">{{ value }}</div>
          <div class="text-sm text-gray-700">{{ row.occupation }}</div>
        </div>
      </div>
    </template>

    <template #column-status="{ value }">
      <StatusTag :status="value" />
    </template>

    <template #column-actions="{ row }">
      <router-link :to="`/users/${row.id}`">Edit</router-link>
    </template>
  </DataTable>
</template>

<script>
import DataTable from '@/components/DataTable.vue'
import StatusTag from '@/components/StatusTag.vue'

export default {
  components: {
    DataTable,
    StatusTag
  },

  data () {
    return {
      defaultSort: 'name',

      columns: [
        { name: 'name', label: 'Name', sortable: true },
        { name: 'email', label: 'Email', sortable: true },
        { name: 'status', label: 'Status', align: 'center', sortable: true },
        { name: 'actions', label: 'Actions', align: 'center' }
      ],

      people: [
        {
          id: 1,
          name: 'Arthur Dent',
          occupation: 'BBC Radio Employee',
          email: 'arthur.dent@bbc.co.uk',
          avatar_url: '/avatars/arthur-dent.jpg',
          status: 'active'
        },
        {
          id: 2,
          name: 'Ford Prefect',
          occupation: 'Researcher',
          email: 'ford.prefect@megadodo.com',
          avatar_url: '/avatars/ford-prefect.jpg',
          status: 'inactive'
        },
        {
          id: 3,
          name: 'Trillian Astra',
          occupation: 'Astrophysicist',
          email: 'trillian.astra@subetha.net',
          avatar_url: '/avatars/trillian-astra.jpg',
          status: 'active'
        }
      ]
    }
  },

  computed: {
    sort () {
      return this.$route.query.sort || this.defaultSort
    },

    sortColumnName () {
      return this.sort.replace(/^-/, '')
    },

    sortDirection () {
      return /^-/.test(this.sort) ? 'desc' : 'asc'
    },

    sortedPeople () {
      return [...this.people].sort((a, b) => {
        a = a[this.sortColumnName].toLowerCase()
        b = b[this.sortColumnName].toLowerCase()

        if (a === b) {
          return 0
        }

        return (a > b ? 1 : -1) * (this.sortDirection === 'asc' ? 1 : -1)
      })
    },
  },

  methods: {
    updateSort (sort) {
      this.$router.replace({ ...this.$route, query: { ...this.$route.query, sort } })
    }
  }
}
</script>
