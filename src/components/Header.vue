<template>
    <div>
      <b-nav>
        <b-nav-item disabled> <b> Quiz App </b> </b-nav-item>
        <b-nav-item disabled>Counter: {{ correctAns }} / {{ total }} </b-nav-item>
      </b-nav>
      <hr>
    </div>
    
</template>

<script>
export default {
    name:"Header",
    props:[
      'correctAns',
      'total'
    ]
}
</script>