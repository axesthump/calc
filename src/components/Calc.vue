<template>
	<div class="calc">
		<div class="calc__field">{{ result }}</div>
		<ul class="calc__numbers">
			<li class="calc__item calc__item--nums" @click="addNum(1)"><div class="rad">1</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(2)"><div class="rad">2</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(3)"><div class="rad">3</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(4)"><div class="rad">4</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(5)"><div class="rad">5</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(6)"><div class="rad">6</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(7)"><div class="rad">7</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(8)"><div class="rad">8</div></li>
			<li class="calc__item calc__item--nums" @click="addNum(9)"><div class="rad">9</div></li>
			<li class="calc__item calc__item--nums calc__item--nums-0" @click="addNum(0)"><div class="rad">0</div></li>
		</ul>
		<ul class="calc__sym">
			<li class="calc__item calc__item--sym" @click="addNum('+')"><span class="calc__item--mar">+</span></li>
			<li class="calc__item calc__item--sym" @click="addNum('-')"><span class="calc__item--mar">-</span></li>
			<li class="calc__item calc__item--sym" @click="addNum('/')"><span class="calc__item--mar">/</span></li>
			<li class="calc__item calc__item--sym" @click="addNum('*')"><span class="calc__item--mar">*</span></li>
			<li class="calc__item calc__item--sym" @click="answer()"><span class="calc__item--mar">=</span></li>
			<li class="calc__item calc__item--sym" @click="clear()"><span class="calc__item--mar">C</span></li>
		</ul>
	</div>
</template>

<script>
export default {
	data() {
    return {
      result: '',
    }
  },
	methods: {
		addNum(num) {
			if (this.result == '') {
				if (!this.isSymb(num)) {
					this.result = '' + num;
				} else {
					alert('Введите число, а не символ!');
				}
			} else {
				var lastSymb = this.result[this.result.length - 1];
				if (this.isSymb(lastSymb) && !this.isSymb(num) || !this.isSymb(lastSymb)) {
					this.result += ' ' + num;
				} else {
					alert('Введите число, а не символ!');
					this.result += '';
				}
			}
		},

		isSymb (symb) {
			if (symb != '-' && symb != '+' && symb != '*' && symb != '/') {
				return false;
			} else {
				return true;
			}
		},

		answer() {
			var an = this.result.split(' ');
			var allNums = [];
			var allSym = [];
			var elem = 0;
			/*Достаем все числа*/
			for (var i = 0; i < an.length; i++) {
				if (this.isSymb(an[i])) {
					allNums.push(an.slice(elem, i).join(''));
					elem = i + 1;
				}
			}
			allNums.push(an.slice(elem, an[an.length]).join(''));
			/*Достаем все символы*/
			for (var i = 0; i < an.length; i++) {
				if (this.isSymb(an[i])) {
					allSym.push(an[i]);
				}
			}

			var operations = [];
			for (var i = 0; i < allSym.length; i++) {
				operations[i] = [allNums[i], allNums[i+1]];
			}

			/*Выполняем операцию умножения*/
			for (var i = 0; i < allSym.length; i++) {
				if (allSym[i] == "*") {
					if (operations[i-1] != undefined) {
						operations[i-1][1] = operations[i][0] * operations[i][1];
					}
					if (operations[i+1] != undefined) {
						operations[i+1][0] = operations[i][0] * operations[i][1];
					}
					if (operations[i-1] == undefined && operations[i+1] == undefined) {
						console.log('Сработало умнажение финальное')
						this.result = operations[i][0] * operations[i][1];
					}
					operations.splice(i, 1);
					allSym.splice(i, 1);
				}
			}

			/*Выполняем операцию деления*/
			for (var i = 0; i < allSym.length; i++) {
				if (allSym[i] == "/") {
					if (operations[i-1] != undefined) {
						operations[i-1][1] = operations[i][0] / operations[i][1];
					}
					if (operations[i+1] != undefined) {
						operations[i+1][0] = operations[i][0] / operations[i][1];
					}
					if (operations[i-1] == undefined && operations[i+1] == undefined) {
						this.result = operations[i][0] / operations[i][1];
					}
					operations.splice(i, 1);
					allSym.splice(i, 1);
					i -= 1;
				}
			}

			/*Выполняем операцию сложения*/
			for (var i = 0; i < allSym.length; i++) {
				if (allSym[i] == "+") {
					if (operations[i-1] != undefined) {
						operations[i-1][1] = Number(operations[i][0]) + Number(operations[i][1]);
					}
					if (operations[i+1] != undefined) {
						operations[i+1][0] = Number(operations[i][0]) + Number(operations[i][1]);
					}
					if (operations[i-1] == undefined && operations[i+1] == undefined) {
						this.result = Number(operations[i][0]) + Number(operations[i][1]);
					}
					operations.splice(i, 1);
					allSym.splice(i, 1);
					i -= 1;
				}
			}

			/*Выполняем операцию вычетания*/
			for (var i = 0; i < allSym.length; i++) {
				if (allSym[i] == "-") {
					if (operations[i-1] != undefined) {
						operations[i-1][1] = Number(operations[i][0]) - Number(operations[i][1]);
					}
					if (operations[i+1] != undefined) {
						operations[i+1][0] = Number(operations[i][0]) - Number(operations[i][1]);
					}
					if (operations[i-1] == undefined && operations[i+1] == undefined) {
						this.result = Number(operations[i][0]) - Number(operations[i][1]);
					}
					operations.splice(i, 1);
					allSym.splice(i, 1);
					i -= 1;
				}
			}
		},

		clear() {
			this.result = '';
		},
	},
}
</script>

<style>
	.calc {
		width: 400px;
		min-height: 183px;

		margin: 0 auto;
		display: flex;
		flex-wrap: wrap;
		align-items: flex-start;
	}

	.calc__numbers {
		width: 150px;

		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;

		list-style: none;

		margin: 0;
		padding: 0;
	}

	.calc__sym {
		width: 200px;

		display: flex;
		flex-wrap: wrap;

		margin: 0;
		padding: 0;
		margin-left: auto;

		list-style: none;

		justify-content: space-between;
	}

	.calc__field {
		width: 100%;
		padding-left: 15px;
		padding-right: 15px;
		border: 1px solid black;
		margin-bottom: 10px;
		font-size: 30px;
		min-height: 96px;

		box-sizing: border-box;

		padding-top: 30px;
	}

	.calc__item {
		text-transform: uppercase;
		width: 20px;
		text-align: center;
		border: 1px solid black;
		margin-bottom: 10px;

		font-size: 30px;

		cursor: pointer;

		border-radius: 10px;
	}

	.calc__item--nums {
		width: 40px;
		font-weight: 700;
		/* text-shadow: 0px 0px 0px black; */

		background: url("../assets/images.jpg") 0 0 no-repeat;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		animation-name: gradient;
		animation-duration: 3s;
		animation-iteration-count: infinite;
	}

	.calc__item--nums:hover {
		width: 45px;
		margin-right: -5px;
		margin-top: -5px;
	}

	.calc__item--nums-0:hover {
		width: 45px;
		margin-left: calc(50% - 25px);
	}

	@keyframes gradient {
		0% {
			background-position: 0 0;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0 0;
		}
	}

	.calc__item--nums:last-child {
		margin-left: calc(50% - 21px);
	}

	.calc__item--sym {
		padding: 0;
		margin: 0;
		padding-left: 20px;
		padding-right: 20px;
		margin-bottom: 10px;

		vertical-align: middle;
	}

	.calc__item--sym:hover {
		color: white;
		background-color: #000;
	}

	.calc__item--mar {
		margin-top: 22px;
		margin-bottom: 22px;
		display: block;
		text-align: center;
	}
</style>