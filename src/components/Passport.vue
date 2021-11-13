<template>
		<div class="mb10">
			<h3 class="m10">Паспортные данные</h3>
			<div class="row longrow mb10">
				<div v-click-outside="hideDropdown">
					<label for="nationality">Гражданство</label>
					<input
            class="input"
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
            class="input"
						type="text"
						id="passportPreNumber"
						v-model="formData.passportPreNumber"
					/>
				</div>
				<div class="row">
					<label for="passportNumber">Номер паспорта</label>
					<input
            class="input"
						type="text"
						id="passportNumber"
						v-model="formData.passportNumber"
					/>
				</div>
				<div class="row">
					<label for="dateOfIssue">Дата выдачи</label>
					<input
            class="input"
						type="date"
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
					<input  class="input" type="text" id="lastNameNR" v-model="formData.lastNameNR" />
				</div>
				<div class="row">
					<label for="nameNR">Name</label>
					<input  class="input" type="text" id="nameNR" v-model="formData.nameNR" />
				</div>
				<div class="row">
					<label for="passportID">passport ID</label>
					<input  class="input" type="text" id="passportID" v-model="formData.passportID" />
				</div>
				<div class="row longrow mb10">
					<div v-click-outside="hideDropdownCountry">
						<label for="countryOfIssue">Country of issue</label>
						<input
             class="input"
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
              class="input"
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
            class="input"
						type="text"
						id="lastNameDataChanged"
						v-model="formData.lastNameDataChanged"
					/>
				</div>
				<div class="row longrow">
					<label for="firstNameDataChanged">Предыдущее Имя</label>
					<input
            class="input"
						type="text"
						id="firstNameDataChanged"
						v-model="formData.firstNameDataChanged"
					/>
				</div>
			</div>
		</div>
</template>

<script>
import ClickOutside from "vue-click-outside";

export default {
    props: {
        formData: {
            type: Object,
            default: null,
        },
        nationalitys: {
            type: Array,
            default: null
        },
        countries: {
            type: Array,
            default: null
        },
        passportTypes: {
            type: Array,
            default: null
        }

    },
    directives: {
	    	ClickOutside,
	  },
    data() {
        return {
            isDropdownOpen: false,
            isDropdownCountryOpen: false,
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
    onPersonalDataNotChanged() {
      	this.formData.lastNameDataChanged = "";
				this.formData.firstNameDataChanged = "";
    }
	},
}
</script>

<style scoped>
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
.input {
	  background-color: #fff;
    border: 2px solid #b3bcc5;
    padding: 4px;
    border-radius: 3px;
    box-sizing: border-box;
    color: #001a34;
}
</style>

