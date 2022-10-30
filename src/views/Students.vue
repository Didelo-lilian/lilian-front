<template>
  <section>
    <div class="cours-template">
      <h1>{{ studentInfoMin?.level }} - {{ capitalize(studentInfoMin?.name) }}</h1>
      <ul class="parent">
        <div v-for="studentLessonMonth in studentLessons?.months" :key="studentLessonMonth.month">
          <li>{{ displayMonth(studentLessonMonth?.month) }} :</li>
          <ul>
            <li v-for="studentLesson in studentLessonMonth.lessons" :key="studentLesson.title"> {{ studentLesson.day }}
              - <a :href="studentLesson.link" alt="{{ studentLesson.title }}">{{ studentLesson.title }}</a></li>
          </ul>
        </div>

        <div>
          <li>Utilitaires:</li>
          <ul>
            <div v-for="(util, index3) in utils" :key="'util-' + index3">
              <li>
                <a :href="util.link">{{ util.name }}</a>
              </li>
            </div>
          </ul>
        </div>
      </ul>
    </div>
  </section>
</template>

<script lang="ts">

/*

<div class="school-template">
			<h1>{{ school?.title }}</h1>
			<ul class="parent">
				<div
					v-for="(content, index) in school?.contents"
					:key="'content-' + index"
				>
					<li>{{ content.title }}</li>
					<ul>
						<li
							v-for="(subContent, index2) in content.subcontents"
							:key="'sub-content-' + index2"
						>
							<a :href="'https://data.lilian.didelo.fr/school/' + school?.level + '/' + realTitle  + '/' + subContent?.link">{{ subContent?.title }}</a>
						</li>
					</ul>
				</div>
			</ul>
		</div>
 */
import {defineComponent, PropType} from "vue";
import _axios from '@/plugins/axios';
import {AxiosResponse} from "axios";

interface StudentInfoMin {
  name: string,
  level: string
}

interface StudentLesson {
  day: string,
  title: string,
  link: string
}

interface StudentLessonMonth {
  month: string,
  lessons: StudentLesson[]
}

interface StudentLessons {
  name: string,
  months: StudentLessonMonth[]
}

interface Util {
  name: string,
  link: string
}

interface DataComponent {
  studentInfoMin: StudentInfoMin | null;
  studentLessons: StudentLessons | null;
  utils: Util[] | null;
  MONTHS: string[];
}

const MONTHS = ["Janvier", "Fevrier", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"]

export default defineComponent({
  name: "Students",

  props: {
    name: {
      type: String as PropType<string>,
      required: true,
      default: "kevin-j",
    },
  },

  data: (): DataComponent => ({
    studentInfoMin: null,
    studentLessons: null,
    utils: null,
    MONTHS: ["Janvier", "Fevrier", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"],
  }),

  mounted() {
    this.loadStudent();
    this.loadUtils();
  },

  watch: {
    name() {
      this.loadStudent();
    },
  },

  methods: {
    capitalize(str: string): string {
      if (str) {
        if (str == "") return "";
        if (str.length === 1) return str.toUpperCase();
        str = str.replaceAll("-", " ");
        return str.charAt(0).toUpperCase() + str.slice(1);
      } else {
        return "";
      }
    },
    loadStudent(): void {
      _axios
          .get('v2/student/' + this.name)
          .then((response: AxiosResponse<StudentInfoMin>) => {
            this.studentInfoMin = response.data;
            this.loadStudentLesson();
          })
    },
    loadStudentLesson(): void {
      _axios
          .get('v2/studentLesson' + this.name)
          .then((response: AxiosResponse<StudentLessons>) => {
            this.studentLessons = response.data;
          })
    },
    loadUtils(): void {
      _axios
          .get(`/v2/studentLesson/utils`)
          .then((response: AxiosResponse<Util[]>) => {
            this.utils = response.data;
          });
    },
    displayMonth(month: string): string {
      if (!month) {
        return "";
      }
      const monthNumber = parseInt(month.split('/')[0]);
      let monthString: string = MONTHS[monthNumber];
      let year: string = month.split('/')[1];
      return monthString + " - " + year;
    }
  },
});
</script>

<style lang="scss" scoped>
section {
  border: 1px solid white;
  overflow: none;

  .parent {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
    grid-gap: 2vh 5vw;
    margin-bottom: 2vh;
  }

  .parent div li {
    width: 100%;
    font-size: 1.5rem;
  }

  h1 {
    text-align: center;
    font-size: 3rem;
  }

  a:hover {
    color: #ff0000;
  }
}
</style>
