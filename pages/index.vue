<template>
    <div>
        <div class="text-center">
            <h1 class="text-4xl tracking-tight font-extrabold text-gray-900 sm:text-5xl md:text-6xl">
                <span class="block text-indigo-600">Les réponses au métier de développeur</span>
            </h1>
            <span class="text-center text-indigo-400 mt-2 font-semibold text-sm italic">(par des développeurs)</span>
            <p class="mt-3 text-center text-gray-500 sm:mt-5 sm:text-lg sm:max-w-xl sm:mx-auto md:mt-5 md:text-xl lg:mx-auto">
                Anim aute id magna aliqua ad ad non deserunt sunt. Qui irure qui lorem cupidatat commodo. Elit sunt amet fugiat veniam occaecat fugiat aliqua.
            </p>
        </div>
        <section class="flex flex-wrap mx-auto my-8 mt-24">
            <NuxtLink
                v-for="question of questions"
                :key="question.slug"
                :to="`/questions/${question.slug}`"
                class="py-8 px-4 lg:w-1/3">
                <div class="h-full flex items-start">
                    <div class="flex-grow pl-6">
                        <h3 class="title-font text-xl font-medium text-gray-900 mb-3">{{ question.title }}</h3>
                        <p class="leading-relaxed mb-5">Photo booth fam kinfolk cold-pressed sriracha leggings jianbing microdosing tousled waistcoat.</p>
                        <a class="inline-flex items-center">
                            <img
                                alt="blog"
                                src="https://dummyimage.com/103x103"
                                class="w-8 h-8 rounded-full flex-shrink-0 object-cover object-center"
                            >
                            <span class="flex-grow flex flex-col pl-3">
                                <span class="title-font font-medium text-gray-900">Alper Kamu</span>
                            </span>
                        </a>
                    </div>
                </div>
            </NuxtLink>
        </section>
    </div>
</template>

<script>
export default {
    async asyncData({ $content }) {
        const answers = await $content('answers')
            .only(['title', 'description', 'img', 'slug', 'author'])
            .sortBy('createdAt', 'desc')
            .fetch()
        const questions = await $content('questions')
            .only(['name', 'title', 'slug'])
            .sortBy('createdAt', 'asc')
            .fetch()
        return {
            answers,
            questions,
        }
    },
    data() {
        return {
            questions: [],
            asnwers: [],
        }
    },
    created() {
        console.log(this.$auth.user);
        // this.getForks();
    },
    methods: {
        async setFork() {
            // Get all users repos
            try { 

                const repos = await this.$axios.$get(`/users/${this.$auth.user.login}/repos`);
                const hasFork = repos.find(repo => repo.name === 'enquete-dev' && repo.fork);

                if(hasFork) {
                    console.log('Has repos');
                } else {
                    await this.$axios.$post(`/repos/khylias/enquete-dev/forks`)
                }
                // If exist create PR, if not create forks of enquete-dev
                // console.log(forks);
            } catch(err) {
                console.log(err);
            }
        }
    }
}
</script>
