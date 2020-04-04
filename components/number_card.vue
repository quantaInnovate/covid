<template>
  <div class="card text-white shadow-sm col">
    <div class="card-body">
      <div class="card-text">
        <div class="row">
          <p class="text-black">{{ topiChospital }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['topiChospital']
  //   data: () => ({
  //     object_topicc: null
  //   }),
}
</script>
