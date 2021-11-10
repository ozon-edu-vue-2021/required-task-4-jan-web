<template>
	<form class="form" @submit.prevent="onSubmitForm">
		<h2>Личные данные</h2>
		<div class="wrapper mb10">
			<div class="row">
				<label for="lastName">Фамилия</label>
				<input type="text" id="lastName" v-model="formData.lastName" />
			</div>
			<div class="row">
				<label for="firstName">Имя</label>
				<input type="text" id="firstName" v-model="formData.firstName" />
			</div>
			<div class="row">
				<label for="patronymic">Отчество</label>
				<input type="text" id="patronymic" v-model="formData.patronymic" />
			</div>
		</div>
		<div class="row longrow mb10">
			<label for="dataOfBirth">Дата рождения</label>
			<input
				type="text"
				id="dataOfBirth"
				placeholder="дд.мм.гггг"
				v-model="formData.dataOfBirth"
			/>
		</div>
		<div class="row mb10">
			<label for="email">E-mail</label>
			<input type="text" id="email" v-model="formData.email" />
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
		<div class="mb10">
			<h3 class="m10">Паспортные данные</h3>
			<div class="row longrow mb10">
				<div v-click-outside="hideDropdown">
					<label for="nationality">Гражданство</label>
					<input
						type="select"
						autocomplete="off"
						id="nationality"
						@focus="isDropdownOpen = true"
						v-model="formData.selectedNationality"
					/>
					<div v-if="isDropdownOpen">
						<div v-if="nationalitys.length">
							<option
								v-for="nationality in nationalitys"
								:key="nationality"
								@click="chooseNationality(nationality)"
							>
								{{ nationality }}
							</option>
						</div>
						<div v-else><option>Ничего не найдено</option></div>
					</div>
				</div>
			</div>
			<div
				class="wrapper mb10"
				v-if="formData.selectedNationality === 'Russia'"
			>
				<div class="row">
					<label for="passportPreNumber">Серия паспорта</label>
					<input
						type="text"
						id="passportPreNumber"
						v-model="formData.passportPreNumber"
					/>
				</div>
				<div class="row">
					<label for="passportNumber">Номер паспорта</label>
					<input
						type="text"
						id="passportNumber"
						v-model="formData.passportNumber"
					/>
				</div>
				<div class="row">
					<label for="dateOfIssue">Дата выдачи</label>
					<input
						type="text"
						id="dateOfIssue"
						placeholder="дд.мм.гггг"
						v-model="formData.dateOfIssue"
					/>
				</div>
			</div>
			<div
				class="wrapper mb10"
				v-else-if="
					formData.selectedNationality !== 'Russia' &&
					formData.selectedNationality
				"
			>
				<div class="row">
					<label for="lastNameNR">Last name</label>
					<input type="text" id="lastNameNR" v-model="formData.lastNameNR" />
				</div>
				<div class="row">
					<label for="nameNR">Name</label>
					<input type="text" id="nameNR" v-model="formData.nameNR" />
				</div>
				<div class="row">
					<label for="passportID">passport ID</label>
					<input type="text" id="passportID" v-model="formData.passportID" />
				</div>
				<div class="row longrow mb10">
					<div v-click-outside="hideDropdownCountry">
						<label for="countryOfIssue">Country of issue</label>
						<input
							type="select"
							autocomplete="off"
							id="countryOfIssue"
							@focus="isDropdownCountryOpen = true"
							v-model="formData.countryOfIssue"
						/>
						<div v-if="isDropdownCountryOpen">
							<div v-if="countries.length">
								<option
									v-for="country in countries"
									:key="country"
									@click="chooseCountry(country)"
								>
									{{ country }}
								</option>
							</div>
							<div v-else><option>Ничего не найдено</option></div>
						</div>
					</div>
				</div>
				<div class="row longrow mb10">
					<div v-click-outside="hideDropdownPassportType">
						<label for="country">Passport type</label>
						<input
							type="select"
							autocomplete="off"
							id="country"
							@focus="isDropdownPassportTypeOpen = true"
							v-model="formData.passportType"
						/>
						<div v-if="isDropdownPassportTypeOpen">
							<div v-if="passportTypes.length">
								<option
									v-for="passportType in passportTypes"
									:key="passportType"
									@click="choosePassportType(passportType)"
								>
									{{ passportType }}
								</option>
							</div>
							<div v-else><option>Ничего не найдено</option></div>
						</div>
					</div>
				</div>
			</div>
			<div class="mb10">
				<h4 class="m0">Меняли ли фамилию или имя?</h4>
				<input
					type="radio"
					id="noDataChanged"
					value="no"
					v-model="formData.isPersonalDataChanged"
					checked
          @change="onPersonalDataNotChanged"
				/><label for="noDataChanged" class="mr10">Нет</label>
				<input
					type="radio"
					id="yesDataChanged"
					value="yes"
					v-model="formData.isPersonalDataChanged"
				/><label for="yesDataChanged">Да</label>
			</div>
			<div class="wrapper mb10" v-if="formData.isPersonalDataChanged === 'yes'">
				<div class="row longrow">
					<label for="lastNameDataChanged">Предыдущая Фамилия</label>
					<input
						type="text"
						id="lastNameDataChanged"
						v-model="formData.lastNameDataChanged"
					/>
				</div>
				<div class="row longrow">
					<label for="firstNameDataChanged">Предыдущее Имя</label>
					<input
						type="text"
						id="firstNameDataChanged"
						v-model="formData.firstNameDataChanged"
					/>
				</div>
			</div>
		</div>
    <input type="submit" value="Отправить данные">
	</form>
</template>

<script>
import ClickOutside from "vue-click-outside";
import Citizenships from "@/assets/data/citizenships.json";
import PassportTypes from "@/assets/data/passport-types.json";
import { pickBy, identity } from 'lodash';

export default {
	directives: {
		ClickOutside,
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
				selectedNationality: ""
			},
			isDropdownOpen: false,
			nationalitys: [],
			countries: [],
			isDropdownCountryOpen: false,
			passportTypes: [],
			isDropdownPassportTypeOpen: false,
		};
	},
	methods: {
		hideDropdown() {
			this.isDropdownOpen = false;
		},
		hideDropdownCountry() {
			this.isDropdownCountryOpen = false;
		},
		hideDropdownPassportType() {
			this.isDropdownPassportTypeOpen = false;
		},
		chooseNationality(nationality) {
      console.log('nationality: ', nationality);
			this.formData.selectedNationality = nationality;
			this.isDropdownOpen = false;
      if(nationality === "Russia") {
        			this.formData.lastNameNR = '';
              this.formData.nameNR = '';
              this.formData.passportID = '';
              this.formData.countryOfIssue = '';
              this.formData.passportType = '';
      } else {
              this.formData.passportPreNumber = '';
              this.formData.passportNumber = '';
              this.formData.dateOfIssue = '';
      }
		},
		chooseCountry(country) {
			this.formData.countryOfIssue = country;
			this.isDropdownCountryOpen = false;
		},
		choosePassportType(passportType) {
			this.formData.passportType = passportType;
			this.isDropdownPassportTypeOpen = false;
		},
    onSubmitForm() {
      const cleanedObject = pickBy(this.formData, identity)
      console.log(cleanedObject);
    },
    onPersonalDataNotChanged() {
      	this.formData.lastNameDataChanged = "";
				this.formData.firstNameDataChanged = "";
    }
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
.m0 {
	margin: 0;
}
.mr10 {
	margin-right: 10px;
}
.space-center {
	justify-content: center;
}
</style>
