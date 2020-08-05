<template>
  <div class="content">
    <button class="add-to-cart" @click="addToCart()">Add to Cart></button>
    <div class="top-row">
      <div class="top part">
        <div class="robot-name">
          {{ selectedRobot.head.title }}
          <span v-if="selectedRobot.head.onSale" class="sale">Sale!</span>
        </div>
        <img :src="selectedRobot.head.src" title="head" />
        <button @click="selectNextHead()" class="prev-selector">&#9668;</button>
        <button @click="selectPreviousHead()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img :src="selectedRobot.leftArm.src" title="left arm" />
        <button @click="selectNextLeftArm()" class="prev-selector">&#9650;</button>
        <button @click="selectPreviousLeftArm()" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img :src="selectedRobot.torso.src" title="left arm" />
        <button @click="selectNextTorsos()" class="prev-selector">&#9668;</button>
        <button @click="selectPreviousTorsos()" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img :src="selectedRobot.rightArm.src" title="left arm" />
        <button @click="selectNextRightArm()" class="prev-selector">&#9650;</button>
        <button @click="selectPreviousRightArm()" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img :src="selectedRobot.base.src" title="left arm" />
        <button @click="selectNextBases()" class="prev-selector">&#9668;</button>
        <button @click="selectPreviousBases()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div>
      <h1>Cart</h1>
      <table>
        <thead>
          <tr>
            <th>Robot</th>
            <th class="cost">Cost</th>
          </tr>
        </thead>
        <tbody>
          <tr :key="index" v-for="(robot, index) in cart">
            <td>{{ robot.head.title }}</td>
            <td>{{ robot.cost }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import avaiableParts from "../data/parts";

function getPreviousValidIndex(index, length) {
  const deprecatedIndex = index - 1;
  return deprecatedIndex < 0 ? length - 1 : deprecatedIndex;
}

function getNextValidIndex(index, length) {
  const incrementedIndex = index + 1;
  return incrementedIndex > length ? 0 : incrementedIndex;
}

export default {
  name: "RobotBuilder",
  data() {
    return {
      cart: [],
      avaiableParts,
      selectHeadIndex: 0,
      selectLeftArmIndex: 0,
      selectTorsosIndex: 0,
      selectRightArmIndex: 0,
      selectBasesIndex: 0,
    };
  },
  computed: {
    selectedRobot() {
      return {
        head: avaiableParts.heads[this.selectHeadIndex],
        leftArm: avaiableParts.arms[this.selectLeftArmIndex],
        torso: avaiableParts.torsos[this.selectTorsosIndex],
        rightArm: avaiableParts.arms[this.selectRightArmIndex],
        base: avaiableParts.bases[this.selectBasesIndex],
      };
    },
  },
  methods: {
    addToCart() {
      const robot = this.selectedRobot;
      const cost = robot.head.cost + robot.leftArm.cost + robot.torso.cost + robot.rightArm.cost + robot.base.cost;
      this.cart.push(Object.assign({}, robot, { cost }));
    },
    selectNextHead() {
      this.selectHeadIndex = getNextValidIndex(this.selectHeadIndex, avaiableParts.heads.length);
    },
    selectNextLeftArm() {
      this.selectLeftArmIndex = getNextValidIndex(this.selectLeftArmIndex, avaiableParts.arms.length);
    },
    selectNextTorsos() {
      this.selectTorsosIndex = getNextValidIndex(this.selectTorsosIndex, avaiableParts.torsos.length);
    },
    selectNextRightArm() {
      this.selectRightArmIndex = getNextValidIndex(this.selectRightArmIndex, avaiableParts.arms.length);
    },
    selectNextBases() {
      this.selectBasesIndex = getNextValidIndex(this.selectBasesIndex, avaiableParts.bases.length);
    },
    selectPreviousHead() {
      this.selectHeadIndex = getPreviousValidIndex(this.selectHeadIndex, avaiableParts.heads.length);
    },
    selectPreviousLeftArm() {
      this.selectLeftArmIndex = getPreviousValidIndex(this.selectLeftArmIndex, avaiableParts.arms.length);
    },
    selectPreviousTorsos() {
      this.selectTorsosIndex = getPreviousValidIndex(this.selectTorsosIndex, avaiableParts.torsos.length);
    },
    selectPreviousRightArm() {
      this.selectRightArmIndex = getPreviousValidIndex(this.selectRightArmIndex, avaiableParts.arms.length);
    },
    selectPreviousBases() {
      this.selectBasesIndex = getPreviousValidIndex(this.selectBasesIndex, avaiableParts.bases.length);
    },
  },
};
</script>

<style scoped>
.part {
  position: relative;
  width: 165px;
  height: 165px;
  border: 3px solid #aaa;
}
.part img {
  width: 165px;
}
.top-row {
  display: flex;
  justify-content: space-around;
}
.middle-row {
  display: flex;
  justify-content: center;
}
.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}
.head {
  border-bottom: none;
}
.left {
  border-right: none;
}
.right {
  border-left: none;
}
.left img {
  transform: rotate(-90deg);
}
.right img {
  transform: rotate(90deg);
}
.bottom {
  border-top: none;
}
.prev-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector,
.center .next-selector {
  opacity: 0.8;
}
.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  right: -3px;
}

.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}

.content {
  position: relative;
}

.add-to-cart {
  position: relative;
  right: 30px;
  width: 220px;
  padding: 3px;
  font-size: 16px;
}

td,
th {
  text-align: left;
  padding: 5px;
  padding-right: 20px;
}

.cost {
  text-align: right;
}
</style>