<template>
  <div class="questions">
    <p>When did Facebook launch?</p>
    <ul>
      <li><a href="">2005</a></li>
      <li><a href="">2008</a></li>
      <li><a href="">2003</a></li>
      <li><a href="">2004</a></li>
    </ul>
    <button type="button" class="nextBtn">
      Next <i class="fa-solid fa-angle-right"></i>
    </button>
  </div>
</template>

<script setup>
</script>

<style lang="scss">
* {
  --tw-ring-offset-shadow: 4px 4px #0000;
  --tw-ring-shadow: 4px 4px #0000;
}

.questions {
  width: 600px;
  max-width: 650px;
  background-color: #fff;
  padding: 40px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  border: none;
  border-radius: 10px;
  --tw-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);
  box-shadow: var(--tw-ring-offset-shadow, 4px 4px #0000),
    var(--tw-ring-shadow, 4px 4px #0000), var(--tw-shadow);

  p {
    font-size: 27px;
    font-weight: 700;
  }

  ul {
    li {
      list-style: none;
      margin: 18px 0;
      border: 1px solid lightgray;
      border-radius: 8px;
      padding: 11px 21px;
      background-color: rgb(251, 251, 251);

      &:hover {
        background-color: rgb(240, 240, 240);
        cursor: pointer;
      }

      a {
        text-decoration: none;
        color: #333;
      }
    }
  }

  .nextBtn {
    cursor: pointer;
    width: 15%;
    align-self: flex-end;
    background-color: rgb(182, 63, 42);
    border: none;
    border-radius: 15px;
    padding: 8px 9px;
    color: #fff;
    font-size: 15px;

    &:hover {
      opacity: 0.95;
    }
  }
}
</style>