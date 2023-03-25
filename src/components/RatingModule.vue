<script setup>
import { ref } from 'vue'
import axios from 'axios'
import SvgStarEmpty from '../assets/SvgStarEmpty.vue'
import SvgStarFilled from '../assets/SvgStarFilled.vue'
import { useNotification } from '@kyvg/vue3-notification'

const { notify } = useNotification()

const stars = ref([1, 2, 3, 4, 5])
const rating = ref(0)
const comment = ref('')
const email = ref('')

async function submitForm() {
	if (email.value && rating.value) {
		try {
			const { data } = await axios.get('https://beta.elcomspb.ru/api/test')
			data.res === 'success'
				? notify({
						type: 'success',
						title: '<h2>Успех!</h2>',
						text: '<h3>Всё отлично!</h3>'
				  })
				: reject()
		} catch (e) {
			notify({
				type: 'error',
				title: '<h2>Ошибка</h2>',
				text: '<h3>Ошибочка вышла...</h3>'
			})
		}
		email.value = ''
		rating.value = 0
		comment.value = ''
	}
}
</script>

<template>
	<div class="wrapper">
		<h2 class="wrapper-header">
			Пожалуйста, оцените удобство работы с приложением
		</h2>
		<div class="star">
			<div
				v-for="star in stars"
				:key="star"
				@click="rating = star"
				class="star-item"
				:class="{ 'star-item--filled': star <= rating }"
			>
				<template v-if="star <= rating">
					<svg-star-filled />
				</template>
				<template v-else>
					<svg-star-empty />
				</template>
			</div>
		</div>
		<form class="form">
			<input
				type="email"
				placeholder="Введите электронный адрес"
				v-model.trim="email"
			/>
			<textarea
				v-if="rating < 5"
				v-model="comment"
				placeholder="Поделитесь мыслями, что можно было бы улучшить (необязательно)"
			/>
			<button type="submit" @click.prevent="submitForm">Отправить отзыв</button>
		</form>
	</div>
</template>

<style lang="scss">
.wrapper {
	margin-inline: auto;
	max-width: 50vw;
	font-family: Calibri, sans-serif;

	&-header {
		text-align: center;
	}

	.star {
		display: flex;
		justify-content: center;
		margin-bottom: 20px;

		&-item {
			width: 20px;
			cursor: pointer;

			svg {
				max-width: 100%;
				transition: 0.3s;

				&:hover {
					filter: drop-shadow(0 0 2px gold);
				}
			}

			&--filled {
				path {
					fill: gold;
				}
			}
		}
	}

	.form {
		display: flex;
		flex-direction: column;
		gap: 15px;

		input,
		button,
		textarea {
			padding: 10px 15px;
			outline: unset;
			border-radius: 5px;
			border: 1px solid #e4e4e4;
			font-size: 16px;
			font-family: Arial, sans-serif;
			resize: vertical;
		}

		button {
			background-color: aquamarine;
			cursor: pointer;
			transition: all 0.3s;
			color: #fff;
			&:hover {
				background-color: #4ca3b9;
			}
		}
	}
}
</style>
