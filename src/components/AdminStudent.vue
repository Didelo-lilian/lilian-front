<template>
	<div class="admin-student">
		<section>
			<h2>Ajouter un étudiant</h2>
			<form>
				<label for="student">
					<span>Nom de l'étudiant</span>
					<input type="text" name="student" v-model="student" required />
				</label>
				<label for="classe">
					<span>Classe</span>
					<input type="text" name="classe" v-model="classe" required />
				</label>
				<button type="submit" @click="submitCreateStudent">Click Here</button>
			</form>
		</section>
		<hr />
		<section>
			<h2>Modifier un étudiant</h2>
			<form>
				<label for="student">
					<span>Nom de l'étudiant</span>
					<select name="student" v-model="studentSelected">
						<option
							v-for="studentName in studentsName"
							:value="studentName"
							:key="studentName"
						>
							{{ studentName }}
						</option>
					</select>
				</label>
				<label for="month">
					<span>Mois</span>
					<input type="text" name="month" v-model="month" required />
				</label>
        <label for="monthInt">
          <span>Mois(int)</span>
          <input type="int" name="monthInt" v-model="monthInt" required />
        </label>
				<label for="day">
					<span>Jour</span>
					<input type="text" name="day" v-model="day" required />
				</label>
        <label for="isInterne">
          <span>Est-ce-que le fichier est présent dans lilian-data ?</span>
          <input type="checkbox" name="isInterne" v-model="isInterne" />
        </label>
        <label for="isCours">
          <span>Est-ce-que le fichier est un cours ?</span>
          <input type="checkbox" name="isCours" v-model="isCours" />
        </label>
				<label for="link">
					<span>Lien</span>
					<input type="text" name="link" v-model="link" />
				</label>
				<label for="title">
					<span>Titre</span>
					<input type="text" name="title" v-model="title" required />
				</label>
				<button type="submit" @click="submitUpdateStudent">Click Here</button>
			</form>
		</section>
		<hr />
    <section>
      <h2>Supprimer la dernière lecon d'un étudiant</h2>
      <form>
        <label for="student">
          <span>Nom de l'étudiant</span>
          <select name="student" v-model="studentSelected">
            <option
                v-for="studentName in studentsName"
                :value="studentName"
                :key="studentName"
            >
              {{ studentName }}
            </option>
          </select>
        </label>
        <button type="submit" @click="submitDeleteLastLesson">Click Here</button>
      </form>
    </section>
    <hr>
		<section>
			<h2>Supprimer un étudiant</h2>
			<form>
				<label for="student">
					<span>Nom de l'étudiant</span>
					<select name="student" v-model="studentSelectedDeleted">
						<option
							v-for="studentName in studentsName"
							:value="studentName"
							:key="studentName"
						>
							{{ studentName }}
						</option>
					</select>
				</label>
				<button type="submit" @click="submitDeleteStudent">Click Here</button>
			</form>
		</section>
		<hr />
    <section>
      <h2>Modifier la classe d'un étudiant</h2>
      <form>
        <label for="student">
          <span>Nom de l'étudiant</span>
          <select name="student" v-model="studentSelected">
            <option
                v-for="studentName in studentsName"
                :value="studentName"
                :key="studentName"
            >
              {{ studentName }}
            </option>
          </select>
        </label>
        <label for="classe">
          <span>Classe</span>
          <input type="text" name="classe" v-model="newClasse" required />
        </label>
        <button type="submit" @click="submitUpdateStudentClass">Click Here</button>
      </form>
    </section>
    <hr>
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { AxiosResponse } from "axios";
import _axios from "@/plugins/axios";

interface Student {
	_id: string;
	name: string;
	class: string;
	cours: {
		_id: string;
		__v: number;
		month: string;
		lessons: {
			_id: string;
			day: string;
			title: string;
			link: string;
		}[];
	}[];
}

interface DataComponent {
	studentsName: string[];
	student: string;
	month: string;
  monthInt: number;
	classe: string;
	day: string;
	link: string;
	title: string;
	studentSelected: string;
  isInterne: boolean;
  isCours: boolean;
	studentSelectedDeleted: string;
  newClasse: string;
}

export default defineComponent({
	name: "AdminStudent",
	data(): DataComponent {
		return {
			studentsName: [],
			student: "",
			month: "",
      monthInt: 0,
			classe: "",
			day: "",
			link: "",
			title: "",
			studentSelected: "",
      isInterne: false,
      isCours: false,
			studentSelectedDeleted: "",
      newClasse: "",
		};
	},
	methods: {
		verify(): void {
			if (localStorage.getItem("token") === null) {
				alert("Vous n'êtes pas connecté");
				setTimeout(() => {
					window.location.href = "/login";
				}, 5000);
				return;
			}
			if (localStorage.getItem("expires_at") != null) {
				if (
					new Date(localStorage.getItem("expires_at") as string) < new Date()
				) {
					alert("Votre session a expiré");
					setTimeout(() => {
						window.location.href = "/login";
					}, 5000);
					return;
				}
			}
		},
		loadStudentsName(): void {
			this.verify();
			this.studentsName = [];
			_axios
				.get("student")
				.then(
					(
						response: AxiosResponse<{ message: string; students: Student[] }>
					) => {
						response.data.students.forEach((student: Student) => {
							this.studentsName?.push(student.name);
						});
						this.student = "";
						this.classe = "";
					}
				);
		},
		submitCreateStudent(e: Event): void {
			e.preventDefault();
			this.verify();
			_axios
				.post(
					"student",
					{ name: this.student, class: this.classe },
					{
						headers: {
							Authorization: `Bearer ${localStorage.getItem("token")}`,
						},
					}
				)
				.then((response: AxiosResponse) => {
					console.log(response);
					alert("Ajouté !");
					this.loadStudentsName();
				})
				.catch((error: Error) => {
					alert("Erreur !");
					console.log(error);
				});
		},
		submitUpdateStudent(e: Event): void {
			e.preventDefault();
			this.verify();
      let newLink = this.link;
      let newTitle = this.title;
      if(this.isInterne){
        newLink = "cours/" + this.studentSelected + "/";
        if(this.isCours) {
          newLink+= this.day + "-" + this.monthInt + "-" + this.studentSelected + ".pdf";
          newTitle = "Cours - " + this.title;
        } else {
          newLink += this.link;
        }
      }
			_axios
				.put(
					`student`,
					{
						name: this.studentSelected,
						month: this.month,
						lessons: {
							day: this.day,
							title: newTitle,
							link: newLink,
						},
					},
					{
						headers: {
							Authorization: `Bearer ${localStorage.getItem("token")}`,
						},
					}
				)
				.then((response: AxiosResponse) => {
					console.log(response);
					alert("Modifié !");
					this.month = "";
					this.day = "";
					this.title = "";
					this.link = "";
				})
				.catch((error: Error) => {
					console.log(error);
					alert("Erreur !");
				});
		},
		submitDeleteStudent(e: Event): void {
			e.preventDefault();
			this.verify();
			_axios
				.delete(`student`, {
					data: {
						name: this.studentSelectedDeleted,
					},
					headers: {
						Authorization: `Bearer ${localStorage.getItem("token")}`,
					},
				})
				.then((response: AxiosResponse) => {
					console.log(response);
					alert("Supprimé !");
					this.loadStudentsName();
				})
				.catch((error: Error) => {
					alert("Erreur !");
					console.log(error);
				});
		},
    submitUpdateStudentClass(e: Event): void {
      e.preventDefault();
      this.verify();
      _axios
        .put(
          `student/class`,
          {
            name: this.studentSelected,
            class: this.newClasse,
          },
          {
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`,
            },
          }
        )
        .then((response: AxiosResponse) => {
          console.log(response);
          alert("Modifié !");
          this.loadStudentsName();
        })
        .catch((error: Error) => {
          alert("Erreur !");
          console.log(error);
        });
    },
    submitDeleteLastLesson(e: Event): void {
      e.preventDefault();
      this.verify();
      _axios
        .delete(
          `student/lesson`,
          {
            data: {
              name: this.studentSelected,
            },
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`,
            },
          }
        )
        .then((response: AxiosResponse) => {
          console.log(response);
          alert("Supprimé !");
        })
        .catch((error: Error) => {
          alert("Erreur !");
          console.log(error);
        });
    },
	},
	mounted() {
		this.loadStudentsName();
	},
});
</script>

<style scoped lang="scss">
.admin-student {
	width: 80vw;
	margin: 0 auto;
	section {
		width: 60vw;
		margin: auto 20vh;
		h2 {
			font-size: 2em;
			text-align: center;
			margin-bottom: 1vh;
		}
		form {
			width: max-content;
			margin: 0 auto;
			display: flex;
			flex-direction: column;
			font-size: 1em;
			label {
				margin-bottom: 2vh;
				span {
					margin-right: 0.5vw;
				}
			}
			button {
				margin-top: 2vh;
				width: fit-content;
				align-self: center;
			}
		}
	}
	hr {
		display: block;
		border: 1px solid white;
		width: 40vw;
		margin-top: 5vh;
		margin-bottom: 5vh;
		text-align: center;
	}
}
</style>
