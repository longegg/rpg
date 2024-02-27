<template>
    <section class="features">
        <h1>Funksjoner</h1>
        <div class="feature">
            <i class="gg-check"></i>
            <p>Søk på en property</p>
        </div>
        <div class="feature">
            <i class="gg-check"></i>
            <p>Filtrer ulike properties</p>
        </div>
        <div class="feature">
            <i class="gg-check"></i>
            <p>Sorter på dato</p>
        </div> 
        <div class="feature">
            <i class="gg-check"></i>
            <p>Mulighet til å lage en ny linje</p>
        </div>                       
    </section>
</template>

<style scoped>
.features {
  display: flex;
  flex-direction: column;
  gap: 2px;
  /* border-bottom: 1px solid rgba(0,0,0,0.1); */
  padding-bottom: 15px;
}
h1 {
  font-size: 26px;
  margin-bottom: 8px;
}
.feature {
    display: flex;
    gap: 5px;
}
</style>
