<template>
	<form class="form" @submit.prevent="onSubmitForm">
		<h2>Личные данные</h2>
		<div class="wrapper mb10">
			<div class="row">
				<label for="lastName">Фамилия</label>
				<input
					class="input"
					type="text"
					id="lastName"
					v-model="formData.lastName"
				/>
			</div>
			<div class="row">
				<label for="firstName">Имя</label>
				<input
					class="input"
					type="text"
					id="firstName"
					v-model="formData.firstName"
				/>
			</div>
			<div class="row">
				<label for="patronymic">Отчество</label>
				<input
					class="input"
					type="text"
					id="patronymic"
					v-model="formData.patronymic"
				/>
			</div>
		</div>
		<div class="row longrow mb10">
			<label for="dataOfBirth">Дата рождения</label>
			<input
				class="input"
				type="date"
				id="dataOfBirth"
				placeholder="дд.мм.гггг"
				v-model="formData.dataOfBirth"
			/>
		</div>
		<div class="row mb10">
			<label for="email">E-mail</label>
			<input class="input" type="email" id="email" v-model="formData.email" />
		</div>
		<div class="mb10">
			<h3 class="m0">Пол</h3>
			<input
				type="radio"
				id="male"
				value="male"
				v-model="formData.sex"
				checked
			/><label for="male" class="mr10">Мужской</label>
			<input
				type="radio"
				id="female"
				value="female"
				v-model="formData.sex"
			/><label for="female">Женский</label>
		</div>
		<passport-data
			:formData="formData"
			:nationalitys="nationalitys"
			:countries="countries"
			:passportTypes="passportTypes"
		/>
		<div class="text-right mt70">
			<input class="button" type="submit" value="Отправить" />
		</div>
	</form>
</template>

<script>
import Citizenships from "@/assets/data/citizenships.json";
import PassportTypes from "@/assets/data/passport-types.json";
import { pickBy, identity } from "lodash";
import PassportData from "./Passport.vue";

export default {
	components: {
		PassportData,
	},
	data() {
		return {
			formData: {
				lastName: "",
				firstName: "",
				patronymic: "",
				dataOfBirth: "",
				email: "",
				sex: "male",
				nationality: "",
				passportPreNumber: "",
				passportNumber: "",
				dateOfIssue: "",
				lastNameNR: "",
				nameNR: "",
				passportID: "",
				countryOfIssue: "",
				passportType: "",
				isPersonalDataChanged: "no",
				lastNameDataChanged: "",
				firstNameDataChanged: "",
				selectedNationality: "",
			},
			nationalitys: [],
			countries: [],
			passportTypes: [],
		};
	},
	methods: {
		onSubmitForm() {
			const cleanedObject = pickBy(this.formData, identity);
			console.log(cleanedObject);
		},
	},

	mounted() {
		this.nationalitys = Array.from(
			new Set(Citizenships.map((item) => item.nationality))
		);
		this.countries = Array.from(new Set(Citizenships.map((item) => item.flag)));
		this.passportTypes = Array.from(
			new Set(PassportTypes.map((item) => item.type))
		);
	},
};
</script>

<style scoped>
.form {
	border-radius: 6px;
	border: 1px solid black;
	background: white;
	max-width: 600px;
	margin: 0 auto;
	padding: 40px;
	overflow: auto;
}
.input {
	background-color: #fff;
	border: 2px solid #b3bcc5;
	border-radius: 3px;
	box-sizing: border-box;
	color: #001a34;
	padding: 4px;
}

.wrapper {
	display: flex;
	justify-content: space-between;
	flex-wrap: wrap;
}
.row {
	display: flex;
	flex-direction: column;
	width: 30%;
}
.longrow {
	width: 48%;
}
.mb10 {
	margin-bottom: 10px;
}
.mt70 {
	margin-top: 70px;
}
.m0 {
	margin: 0;
}
.mr10 {
	margin-right: 10px;
}
.text-right {
	text-align: right;
}
.space-center {
	justify-content: center;
}
.button {
	font-size: 16px;
	line-height: 1.25;
	min-height: 56px;
	padding: 0 24px;
	border-radius: 6px;
	align-items: center;
	background-color: #005bff;
	box-sizing: border-box;
	color: #fff;
}
</style>
