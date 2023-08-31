<script setup lang="ts">
import Block from './components/Block.vue';
import { ref } from 'vue';

class GameOfLive{
  private _blocks = ref([] as boolean[][])
  width = 250;
  height = 120;

  constructor(){
    for(let i = 0; i < this.height; i++){
      this.blocks[i] = Array(this.width).fill(false);
    }
    this.randomInit();
    window.setInterval(()=>{
      this.run();
    },1500)
  }

  randomInit(lives=2500){
    while(lives--){
      const i = Math.floor(Math.random()*this.height),
      j = Math.floor(Math.random()*this.width);
      this.blocks[i][j] = true;
    }
  }

  run(){
    const buffer:[number,number,boolean][] = [];
    for(let i = 0; i < this.height; i++){
      for(let j = 0; j < this.width; j++){
        const alives = this.neighbours(i,j);
        if(alives===3&&!this.blocks[i][j])
          buffer.push([i,j,true]);
        if(alives<=1&&this.blocks[i][j])
          buffer.push([i,j,false]);
        else if(alives<=3)
          continue;
        else 
          buffer.push([i,j,false]);
      }
    }

    for(const unit of buffer){
      this.blocks[unit[0]][unit[1]] = unit[2];
    }
  }

  neighbours(row:number, col:number):number{
    let res = 0;
    for(let i = Math.max(row-1,0);i<=Math.min(row+1,this.height-1);++i){
      for(let j = Math.max(col-1,0);j<=Math.min(col+1,this.width-1);++j){
        if(row===i&&col===j)
          continue;
        res += this.blocks[i][j]? 1 : 0;
      }
    }
    return res;
  }

  get blocks(){
    return this._blocks.value;
  }

  set blocks(blocks:boolean[][]){
    this._blocks.value = blocks;
  }
}

const control = new GameOfLive();
</script>

<template>
  <div class="main">
    <div class="line" v-for="line in control.blocks">
      <Block v-for="alive in line" :is-alive="alive"/>
    </div>
  </div>
</template>

<style scoped>
.main{
  height: 100vh;
  width: 100vw;
  background-color: black;
}

.line{
  display: flex;
}
</style>
