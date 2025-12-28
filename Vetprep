<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>VetPrep | 亞大學士後獸醫考古題練習</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&display=swap');
        body { font-family: 'Noto Sans TC', sans-serif; background-color: #F5F5F0; -webkit-tap-highlight-color: transparent; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .passage-box { background-color: #fdfbf7; border: 1px solid #e5e7eb; border-radius: 12px; margin-bottom: 20px; overflow: hidden; box-shadow: inset 0 2px 4px rgba(0,0,0,0.05); }
        .passage-header { background-color: #f3f4f6; padding: 8px 16px; font-size: 12px; font-weight: bold; color: #6b7280; border-bottom: 1px solid #e5e7eb; }
        .passage-content { padding: 16px; font-size: 15px; line-height: 1.6; color: #374151; font-family: "Times New Roman", serif; max-height: 250px; overflow-y: auto; }
        .question-text { font-size: 18px; font-weight: bold; margin-bottom: 15px; color: #1f2937; }
        .gecko-bg { position: fixed; bottom: 78px; right: 100px; width: 120px; z-index: 999; pointer-events: none; }


.gecko-bg {
    position: fixed;      
    bottom: 77px;         /* Moved up slightly so it's not cut off */
    right: 90px;          /* Moved left slightly */
    width: 120px;         
    z-index: 999;         /* Changed from -1 to 999 to bring it to the FRONT */
    pointer-events: none; /* IMPORTANT: This lets you click the buttons "through" the image */
}


    </style>
</head>
<body>

<img src="my-gecko.png" alt="Pet Gecko" class="gecko-bg">

    <div id="app" class="max-w-md mx-auto min-h-screen flex flex-col relative bg-white shadow-xl overflow-hidden">
        
        <header class="p-4 bg-[#C3B091] text-white sticky top-0 z-30 flex justify-between items-center shadow-md">
            <div class="flex items-center">
                <button v-if="activeSubject" @click="goBack" class="mr-3">
                    <i class="fas fa-chevron-left"></i>
                </button>
                <i class="fas fa-paw text-white/90"></i> <h1 class="text-xl font-bold italic">VetPrep</h1>
            </div>


            <div v-if="activeYear && !quizFinished" class="bg-black/20 px-3 py-1 rounded-lg font-mono text-sm">
                <i class="far fa-clock mr-1"></i>{{ formatTime(timer) }}
            </div>
            <button v-else @click="resetAllData" class="text-xs bg-white/20 px-2 py-1 rounded">重置</button>
        </header>

        <main class="flex-1 overflow-y-auto p-4 pb-24">
            
            <section v-if="currentTab === 'home'">
                <div class="bg-[#D2B48C] rounded-2xl p-6 text-white shadow-lg mb-6">
                    <p class="text-sm opacity-90">學習總進度</p>
                    <h2 class="text-3xl font-bold mt-1">{{ totalProgress }}%</h2>
                </div>
                <h3 class="font-bold text-gray-700 mb-4 ml-1">科目掌握度</h3>
                <div class="space-y-4">
                    <div v-for="sub in subjects" :key="sub.name" class="bg-white border border-gray-100 p-4 rounded-xl shadow-sm flex items-center">
                        <div :class="sub.color" class="w-10 h-10 rounded-lg flex items-center justify-center text-white mr-4"><i :class="sub.icon"></i></div>
                        <div class="flex-1">
                            <div class="flex justify-between mb-1 text-sm font-medium"><span>{{ sub.name }}</span><span>{{ sub.progress }}%</span></div>
                            <div class="w-full bg-gray-100 h-1.5 rounded-full"><div :class="sub.barColor" class="h-1.5 rounded-full transition-all duration-500" :style="{ width: sub.progress + '%' }"></div></div>
                        </div>
                    </div>
                </div>
            </section>

            <section v-if="currentTab === 'quiz'">
                <div v-if="!activeSubject" class="grid grid-cols-1 gap-4">
                    <button v-for="sub in subjects" @click="selectSubject(sub.name)" class="bg-white p-6 rounded-2xl border-2 border-gray-50 flex items-center justify-between shadow-sm">
                        <div class="flex items-center">
                            <div :class="sub.color" class="w-12 h-12 rounded-xl flex items-center justify-center text-white mr-4"><i :class="sub.icon + ' fa-lg'"></i></div>
                            <div class="font-bold text-gray-800 text-lg">{{ sub.name }}</div>
                        </div>
                        <i class="fas fa-chevron-right text-gray-300"></i>
                    </button>
                </div>

                <div v-else-if="activeSubject && !activeYear" class="space-y-4">
                    <h3 class="font-bold text-gray-700 mb-2 ml-1">選擇 {{ activeSubject }} 考卷年份</h3>
                    <button v-for="year in availableYears" @click="selectYear(year)" class="w-full bg-white p-5 rounded-2xl border border-gray-100 shadow-sm flex justify-between items-center text-left">
                        <div>
                            <span class="font-bold text-gray-700 block">{{ year }} 年 考古題</span>
                            <span v-if="getHistory(activeSubject, year)" class="text-[10px] text-emerald-500 font-bold">
                                上次得分：{{ getHistory(activeSubject, year).score }}% ({{ getHistory(activeSubject, year).date }})
                            </span>
                        </div>
                        <span class="text-xs bg-gray-100 text-gray-400 px-3 py-1 rounded-full whitespace-nowrap">共 {{ getQuestionCount(year) }} 題</span>
                    </button>
                    <div v-if="availableYears.length === 0" class="text-center py-10 text-gray-400">目前尚無此科目題庫</div>
                </div>

                <div v-else-if="!quizFinished">
                    <div v-if="currentQuestion.passage" class="passage-box">
                        <div class="passage-header"><i class="fas fa-file-alt"></i> 閱讀文章</div>
                        <div class="passage-content">{{ currentQuestion.passage }}</div>
                    </div>

                    <div class="relative mb-6 flex justify-between items-center">
                        <button @click="showNavigator = !showNavigator" class="bg-[#C3B091] text-white px-4 py-2 rounded-xl text-sm font-bold shadow-md">
                            {{ currentQuestionIndex + 1 }} / {{ filteredQuestions.length }} <i class="fas fa-th-large ml-1"></i>
                        </button>
                        <button @click="toggleFavorite(currentQuestion)" class="text-2xl">
                            <i :class="isFavorited(currentQuestion.id) ? 'fas fa-star text-yellow-400' : 'far fa-star text-gray-300'"></i>
                        </button>

                        <div v-if="showNavigator" class="absolute top-12 left-0 w-full bg-white border-2 border-[#C3B091] rounded-2xl shadow-2xl z-50 p-4 grid grid-cols-5 gap-2 max-h-64 overflow-y-auto">
                            <button v-for="(q, idx) in filteredQuestions" :key="idx" @click="jumpToQuestion(idx)"
                                :class="currentQuestionIndex === idx ? 'bg-[#C3B091] text-white' : 'bg-gray-100'" class="py-2 rounded-lg font-bold text-xs transition-colors">
                                {{ idx + 1 }}
                            </button>
                        </div>
                    </div>

                    <div class="question-text">
                        <span class="text-[#C3B091] mr-1">Q{{ currentQuestionIndex + 1 }}.</span>
                        {{ currentQuestion.text }}
                    </div>
                    
                    <div class="space-y-3">
                        <button v-for="(option, index) in currentQuestion.options" :key="index" @click="selectOption(index)" :disabled="showFeedback"
                            :class="getOptionClass(index)" class="w-full p-4 rounded-xl border-2 text-left flex justify-between items-center shadow-sm">
                            <span>{{ option }}</span>
                            <i v-if="showFeedback && index === currentQuestion.correct" class="fas fa-check-circle text-emerald-500"></i>
                        </button>
                    </div>

                    <div v-if="showFeedback" class="mt-6">
                        <div class="p-4 bg-[#F9F6F0] border rounded-xl text-sm text-[#82786E] mb-4">{{ currentQuestion.explanation }}</div>
                        <button @click="nextQuestion" class="w-full bg-[#C3B091] text-white py-4 rounded-xl font-bold shadow-lg">下一題</button>
                    </div>
                </div>

                <div v-else class="text-center py-10">
                    <div class="text-5xl font-bold text-[#C3B091] mb-2">{{ Math.round((correctCount / filteredQuestions.length) * 100) }}%</div>
                    <p class="text-gray-500 mb-8">{{ activeYear }}年 {{ activeSubject }} 測驗完成！</p>
                    <button @click="activeYear = null; quizFinished = false;" class="bg-[#C3B091] text-white px-10 py-3 rounded-full font-bold shadow-lg">返回年份列表</button>
                </div>
            </section>

            <section v-if="currentTab === 'favorites' || currentTab === 'errors'">
                <div class="flex bg-gray-100 p-1 rounded-xl mb-4 overflow-x-auto no-scrollbar">
                    <button v-for="sub in subjects" @click="filterSub = sub.name" :class="filterSub === sub.name ? 'bg-white text-[#C3B091] shadow-sm' : 'text-gray-400'" 
                        class="flex-1 py-2 px-3 rounded-lg text-xs font-bold whitespace-nowrap transition-all">{{ sub.name }}</button>
                </div>
                <div v-if="getFilteredItems(currentTab === 'favorites' ? favorites : errorLog).length === 0" class="text-center py-20 text-gray-400 italic">目前沒有紀錄</div>
                <div v-for="item in getFilteredItems(currentTab === 'favorites' ? favorites : errorLog)" :key="item.id" class="bg-white border-l-4 p-4 rounded-xl shadow-sm mb-3" :class="currentTab === 'favorites' ? 'border-yellow-400' : 'border-red-400'">
                    <p class="text-[10px] text-gray-400 mb-1">{{ item.year }}年 - {{ item.subject }}</p>
                    <p class="font-medium text-gray-800">{{ item.text }}</p>
                    <p class="text-xs text-emerald-600 font-bold mt-2">答案：{{ item.options[item.correct] }}</p>
                </div>
            </section>
        </main>

        <nav class="fixed bottom-0 max-w-md w-full bg-white border-t flex justify-around items-center py-2 shadow-lg z-40 safe-area-bottom">
            <button v-for="tab in [{id:'home', icon:'chart-pie', n:'進度'}, {id:'quiz', icon:'pencil-alt', n:'測驗'}, {id:'favorites', icon:'star', n:'收藏'}, {id:'errors', icon:'book-open', n:'錯題'}]" 
                @click="currentTab = tab.id; activeSubject = null; activeYear = null" :class="currentTab === tab.id ? 'text-[#C3B091]' : 'text-gray-400'" class="flex flex-col items-center flex-1">
                <i :class="'fas fa-' + tab.icon" class="text-lg"></i>
                <span class="text-[10px] mt-1 font-bold">{{ tab.n }}</span>
            </button>
        </nav>
    </div>

    <script>
        const { createApp, ref, computed, watch } = Vue;
        createApp({
            setup() {
                const currentTab = ref('home');
                const activeSubject = ref(null);
                const activeYear = ref(null);
                const filterSub = ref('英文');
                const showNavigator = ref(false);
                const quizFinished = ref(false);
                const currentQuestionIndex = ref(0);
                const selectedOption = ref(null);
                const showFeedback = ref(false);
                const correctCount = ref(0);
                const timer = ref(600);
                let timerInterval = null;

                const subjects = ref(JSON.parse(localStorage.getItem('vet_subjects')) || [
                    { name: '英文', progress: 0, icon: 'fas fa-font', color: 'bg-[#D2B48C]', barColor: 'bg-[#D2B48C]' },
                    { name: '生物學', progress: 0, icon: 'fas fa-dna', color: 'bg-[#C3B091]', barColor: 'bg-[#C3B091]' },
                    { name: '生理學', progress: 0, icon: 'fas fa-heartbeat', color: 'bg-[#E2C2B9]', barColor: 'bg-[#E2C2B9]' },
                    { name: '解剖學', progress: 0, icon: 'fas fa-bone', color: 'bg-[#B5C7D3]', barColor: 'bg-[#B5C7D3]' }
                ]);
                const favorites = ref(JSON.parse(localStorage.getItem('vet_favorites')) || []);
                const errorLog = ref(JSON.parse(localStorage.getItem('vet_errors')) || []);
                
                // --- NEW: Record History Storage ---
                const quizHistory = ref(JSON.parse(localStorage.getItem('vet_history')) || {});

                watch([subjects, favorites, errorLog, quizHistory], () => {
                    localStorage.setItem('vet_subjects', JSON.stringify(subjects.value));
                    localStorage.setItem('vet_favorites', JSON.stringify(favorites.value));
                    localStorage.setItem('vet_errors', JSON.stringify(errorLog.value));
                    localStorage.setItem('vet_history', JSON.stringify(quizHistory.value));
                }, { deep: true });

                const allQuestions = ref([
    // --- 第一部分：單句文法與詞彙 (1-60題) ---
    { id: 10601, subject: '英文', year: 106, text: "1. Only when a monkey is mature enough ________ independence from its mother.", options: ["(A) does it begin to develop", "(B) it begins to develop", "(C) it will begin developing"], correct: 0, explanation: "【倒裝句】Only 引導的副詞子句置於句首，主句需倒裝。" },
    { id: 10602, subject: '英文', year: 106, text: "2. A sneeze cannot be performed voluntarily, ________ be easily suppressed.", options: ["(A) nor it can", "(B) it also cannot", "(C) nor can it"], correct: 2, explanation: "【否定倒裝】nor 引導的句子須倒裝。" },
    { id: 10603, subject: '英文', year: 106, text: "3. If we ________, we won't get tired.", options: ["(A) drive by turns", "(B) were to drive by turns", "(C) should drive in turn"], correct: 1, explanation: "【假設語氣】與未來事實相反或極少可能發生。" },
    { id: 10604, subject: '英文', year: 106, text: "4. ________ your support, I would have failed.", options: ["(A) If it were not for", "(B) Had it not been for", "(C) Without no"], correct: 1, explanation: "【假設語氣】與過去事實相反，使用 Had it not been for。" },
    { id: 10605, subject: '英文', year: 106, text: "5. It is necessary that natural resources ________ conserved.", options: ["(A) are", "(B) be", "(C) will be"], correct: 1, explanation: "【虛擬語氣】It is necessary that 後方動詞應使用原形動詞 be (省略 should)。" },
    { id: 10606, subject: '英文', year: 106, text: "6. I ________ abroad last year but for my illness.", options: ["(A) would go", "(B) had gone", "(C) would have gone"], correct: 2, explanation: "【假設語氣】與過去事實相反，使用 would have + p.p.。" },
    { id: 10607, subject: '英文', year: 106, text: "7. ________ from the earth, the galaxy in the far-away heavens looks like a whirlpool.", options: ["(A) To observe", "(B) Observing", "(C) Observed"], correct: 2, explanation: "【分詞構句】星系是被觀察的，使用過去分詞 Observed。" },
    { id: 10608, subject: '英文', year: 106, text: "8. Of all the mammals in the world, ________ eucalyptus leaves.", options: ["(A) the exclusive food... is only", "(B) only the koala bear feeds exclusively on", "(C) the exclusion of..."], correct: 1, explanation: "【動物習性】無尾熊以尤加利葉為食。" },
    { id: 10609, subject: '英文', year: 106, text: "9. The extinction of dinosaurs 65 million years ago can be explained by ________ the impact theory.", options: ["(A) what scientists call", "(B) that scientists call", "(C) which is called"], correct: 0, explanation: "【關係代名詞】what 引導子句作為介系詞受格。" },
    { id: 10610, subject: '英文', year: 106, text: "10. ________ some animals can predict impending earthquakes still mystifies biologists.", options: ["(A) How", "(B) Now that", "(C) That"], correct: 2, explanation: "【名詞子句】That 引導子句作為整句話的主詞。" },
    { id: 10611, subject: '英文', year: 106, text: "11. ________ nearly every part of the tiger has certain 'mysterious' curative effects is a deeply rooted belief among the Chinese people.", options: ["(A) How", "(B) That", "(C) Why"], correct: 1, explanation: "【名詞子句】That 引導子句作為主詞。" },
    { id: 10612, subject: '英文', year: 106, text: "12. ________ its olfactory functions, the elephant's trunk also serves as another 'limb'...", options: ["(A) Except for", "(B) It is added to", "(C) In addition to"], correct: 2, explanation: "【介系詞】In addition to (除了...之外)。" },
    { id: 10613, subject: '英文', year: 106, text: "13. Many parts of Australia are not fit for life ________ lack of water.", options: ["(A) because it", "(B) is because it", "(C) because of its"], correct: 2, explanation: "【介詞用法】because of 後接名詞。" },
    { id: 10614, subject: '英文', year: 106, text: "14. The elephant can bring food to its mouth and even carry heavy objects ________ its trunk.", options: ["(A) because", "(B) by means of", "(C) by means with"], correct: 1, explanation: "【片語】by means of (藉由)。" },
    { id: 10615, subject: '英文', year: 106, text: "15. ________ other reptiles, the lizard lacks a constant body temperature.", options: ["(A) Unlike", "(B) Likelessly", "(C) Not alike"], correct: 0, explanation: "【生理特徵】Unlike (不像)。蜥蜴是變溫動物。" },
    { id: 10616, subject: '英文', year: 106, text: "16. An analgesic is any drug that relieves pain ________ causing unconsciousness.", options: ["(A) no", "(B) without", "(C) not"], correct: 1, explanation: "【藥理單字】analgesic 為止痛藥，指不導致昏迷而緩解疼痛。" },
    { id: 10617, subject: '英文', year: 106, text: "17. The ant, ________ the bee, is a social animal.", options: ["(A) is like", "(B) likely", "(C) like"], correct: 2, explanation: "【介系詞】like (如同)。" },
    { id: 10618, subject: '英文', year: 106, text: "18. Some mammals are so tiny ________ only a few inches in length.", options: ["(A) that measure", "(B) they measure that", "(C) that they measure"], correct: 2, explanation: "【程度句型】so... that... (如此...以至於...)。" },
    { id: 10619, subject: '英文', year: 106, text: "19. Gorillas... usually do not attack humans unless ________ or cornered.", options: ["(A) are provoked", "(B) are they provoked", "(C) provoked"], correct: 2, explanation: "【分詞省略】unless (they are) provoked。" },
    { id: 10620, subject: '英文', year: 106, text: "20. Owls live mainly on small rodents and can be found ________ other species of birds can survive.", options: ["(A) regardless", "(B) somewhere", "(C) wherever"], correct: 2, explanation: "【連接詞】wherever (無論何處)。" },
    { id: 10621, subject: '英文', year: 106, text: "21. ________ is active during the night, it likes to spend the day in hollow trees and to nest there.", options: ["(A) Even the owl", "(B) Although the owl", "(C) The owl"], correct: 1, explanation: "【連接詞】Although (雖然)。" },
    { id: 10622, subject: '英文', year: 106, text: "22. Biologists believe ________ may help some reptiles to escape from their enemies.", options: ["(A) that losing tails", "(B) when losing tails", "(C) the tails losing"], correct: 0, explanation: "【名詞子句】動名詞 losing tails 作為子句主詞。" },
    { id: 10623, subject: '英文', year: 106, text: "23. ________ the symptoms of AIDS do not appear at the time of its contraction.", options: ["(A) Usually", "(B) Usually when they are", "(C) There are usually"], correct: 0, explanation: "【副詞用法】通常情況下。" },
    { id: 10624, subject: '英文', year: 106, text: "24. ________ that forty percent of the oxygen of the world is produced in the Amazon rainforest.", options: ["(A) It is estimated", "(B) It is estimation", "(C) There is estimated"], correct: 0, explanation: "【虛擬主詞】據估計...。" },
    { id: 10625, subject: '英文', year: 106, text: "25. ________ of oxygen and hydrogen, which are two of the trace elements in air.", options: ["(A) Water being composed", "(B) Water that is composed", "(C) Water is composed"], correct: 2, explanation: "【基礎化學】水由氧與氫組成。" },
    { id: 10626, subject: '英文', year: 106, text: "26. ________ us the ability to resist disease, but also helps us build our body tissues.", options: ["(A) Not only does vitamin C provide", "(B) Not only vitamin C provides", "(C) Vitamin C not only provides"], correct: 0, explanation: "【相關連結詞】Not only 置於句首主句須倒裝。" },
    { id: 10627, subject: '英文', year: 106, text: "27. Amphibians are cold-blooded animals... and most of such animals lay eggs ________ or in a moist place.", options: ["(A) water in either", "(B) either in water", "(C) in either water"], correct: 1, explanation: "【連結詞】either... or...。" },
    { id: 10628, subject: '英文', year: 106, text: "28. Koala bears are ________ marsupials.", options: ["(A) leaves-eating", "(B) eating leaves", "(C) leaf-eating"], correct: 2, explanation: "【複合形容詞】名詞(單數)+Ving。" },
    { id: 10629, subject: '英文', year: 106, text: "29. Some ________ animals like giraffes, okapis, and cattle can extend their tongues...", options: ["(A) eating-plants", "(B) plants-eating", "(C) plant-eating"], correct: 2, explanation: "【複合形容詞】plant-eating (食草的)。" },
    { id: 10630, subject: '英文', year: 106, text: "30. ________ unwilling to do so, he had to follow the traditional ways of doing things.", options: ["(A) Because", "(B) Even", "(C) Even though"], correct: 2, explanation: "【連接詞】Even though (雖然)。" },
    { id: 10631, subject: '英文', year: 106, text: "31. Taxonomy is a scientific method which deals with ________.", options: ["(A) all living things are classified", "(B) living things are all classified", "(C) the classification of all living things"], correct: 2, explanation: "【名詞用法】生物分類法。" },
    { id: 10632, subject: '英文', year: 106, text: "32. The adult kangaroo may stand ________ tall as six feet in height.", options: ["(A) as", "(B) that", "(C) so"], correct: 0, explanation: "【比較句型】as... as...。" },
    { id: 10633, subject: '英文', year: 106, text: "33. Some species of baby fish are ________ tiny and transparent that they are almost invisible.", options: ["(A) too", "(B) so", "(C) that"], correct: 1, explanation: "【程度句型】so... that...。" },
    { id: 10634, subject: '英文', year: 106, text: "34. Mangroves are tropical plants that live in the intertidal zone where sea water and fresh water are ________.", options: ["(A) mingled", "(B) drained", "(C) polluted"], correct: 0, explanation: "【環境生態】海水與淡水混合。" },
    { id: 10635, subject: '英文', year: 106, text: "35. Most psychologists deeply believe that it is just as difficult to change a person's way of thinking ________ to rectify his deep-rooted habits.", options: ["(A) than it does", "(B) as it does", "(C) as it is"], correct: 2, explanation: "【比較句型】as... as... 對等結構。" },
    { id: 10636, subject: '英文', year: 106, text: "36. Organ transplants are the most difficult jobs ________ the surgical operations.", options: ["(A) except", "(B) from", "(C) among"], correct: 2, explanation: "【介系詞】among (在...之中)。" },
    { id: 10637, subject: '英文', year: 106, text: "37. The size of birds ________ on their ability to fly, for weight increases its burden...", options: ["(A) imposes a limit", "(B) has imposed a limit", "(C) is imposing a limit"], correct: 0, explanation: "【動詞用法】限制了...。" },
    { id: 10638, subject: '英文', year: 106, text: "38. Alcohol and caffeine can make the stomach more sensitive to the ________ effect of aspirin.", options: ["(A) pacifying", "(B) irritating", "(C) furtive"], correct: 1, explanation: "【醫療英文】irritating (刺激性的)。" },
    { id: 10639, subject: '英文', year: 106, text: "39. Some marine fish can excrete salt by means of ________ of special cells in the gills and intestines.", options: ["(A) clusters", "(B) swarms", "(C) flocks"], correct: 0, explanation: "【單字】clusters (簇/群)。" },
    { id: 10640, subject: '英文', year: 106, text: "40. Aspirin is used to ________ the effect of pains, such as headache...", options: ["(A) lessen", "(B) lessening", "(C) less"], correct: 0, explanation: "【不定詞】to 後接原形動詞。" },
    { id: 10641, subject: '英文', year: 106, text: "41. I want to go to the dentist, but you ________ with me.", options: ["(A) do not need go", "(B) need not to go", "(C) need not go"], correct: 2, explanation: "【助動詞】need not + 原形動詞。" },
    { id: 10642, subject: '英文', year: 106, text: "42. The infant ________ quietly sucking its thumb.", options: ["(A) lied", "(B) lay", "(C) laid"], correct: 1, explanation: "【動詞變化】lie 的過去式 lay (躺)。" },
    { id: 10643, subject: '英文', year: 106, text: "43. The process ________ plants to transform solar energy into food is called photosynthesis.", options: ["(A) it enables", "(B) that enables", "(C) enables"], correct: 1, explanation: "【關係代名詞】that 引導形容詞子句。" },
    { id: 10644, subject: '英文', year: 106, text: "44. Some plants have obvious distinctive features for us to tell them apart at the first sight, while others have ________.", options: ["(A) no", "(B) never", "(C) none"], correct: 2, explanation: "【代名詞】none (一個也沒有/毫無)。" },
    { id: 10645, subject: '英文', year: 106, text: "45. Scientists believe that ________ twins can be identical in every aspect.", options: ["(A) not", "(B) none", "(C) no"], correct: 2, explanation: "【限定詞】no twins (沒有雙胞胎)。" },
    { id: 10646, subject: '英文', year: 106, text: "46. With the outbreak of SARS, how to control this highly contagious disease has become a major ________ in Taiwan.", options: ["(A) affair", "(B) obstacle", "(C) doubt"], correct: 0, explanation: "【單字】affair (事務/事件)。" },
    { id: 10647, subject: '英文', year: 106, text: "47. ________ 150 people were killed in an aircrash that took place in Nigeria...", options: ["(A) Estimated", "(B) An estimate of", "(C) An estimated"], correct: 2, explanation: "【修飾語】An estimated + 數字 (估計約...)。" },
    { id: 10648, subject: '英文', year: 106, text: "48. Frankly speaking, ________ your timely help, I couldn't have dealt with that problem all by myself.", options: ["(A) if it were not for", "(B) if it had not been for", "(C) if there were not for"], correct: 1, explanation: "【假設語氣】與過去事實相反。" },
    { id: 10649, subject: '英文', year: 106, text: "49. Environmentalists ________ to the construction of another nuclear power plant in Taiwan.", options: ["(A) oppose", "(B) object", "(C) offend"], correct: 1, explanation: "【片語】object to (反對)。" },
    { id: 10650, subject: '英文', year: 106, text: "50. A good speaker must be ________ or else his speech is likely to be boring.", options: ["(A) eloquent", "(B) essential", "(C) monotonous"], correct: 0, explanation: "【單字】eloquent (雄辯的/口才流利的)。" },
    { id: 10651, subject: '英文', year: 106, text: "51. Instead of withdrawing into a corner, you should ________ your courage and meet all the challenges...", options: ["(A) set up", "(B) make up", "(C) pluck up"], correct: 2, explanation: "【片語】pluck up one's courage (鼓起勇氣)。" },
    { id: 10652, subject: '英文', year: 106, text: "52. No matter how hard I tried, I just couldn't ________ John into quitting smoking.", options: ["(A) persuade", "(B) talk", "(C) convince"], correct: 1, explanation: "【片語】talk someone into + Ving (說服某人做...)。" },
    { id: 10653, subject: '英文', year: 106, text: "53. I don't trust Mary because what she does hardly ever ________ what she says.", options: ["(A) corresponds to", "(B) abides by", "(C) results in"], correct: 0, explanation: "【片語】corresponds to (與...相符)。" },
    { id: 10654, subject: '英文', year: 106, text: "54. Green turtles ________ the vast beds of sea grasses found throughout the tropics...", options: ["(A) graze on", "(B) sleep on", "(C) dominate"], correct: 0, explanation: "【單字】graze on (吃草/放牧)。" },
    { id: 10655, subject: '英文', year: 106, text: "55. Most rodents, like squirrels, store ________ food for harsh winter months...", options: ["(A) sensitive", "(B) costly", "(C) surplus"], correct: 2, explanation: "【單字】surplus (過剩的/剩餘的)。" },
    { id: 10656, subject: '英文', year: 106, text: "56. Most birds of prey... have the ability to ________ their prey with their keen sight...", options: ["(A) solve", "(B) tear up", "(C) clasp"], correct: 2, explanation: "【單字】clasp (緊握/抓住)。" },
    { id: 10657, subject: '英文', year: 106, text: "57. Energy crops are fast-growing perennial plants that are ________ exclusively for biofuel production.", options: ["(A) squeezed", "(B) harvested", "(C) harassed"], correct: 1, explanation: "【單字】harvested (收割/採收)。" },
    { id: 10658, subject: '英文', year: 106, text: "58. Cold-blooded animals must ________ during the winter months because they lack internal control...", options: ["(A) freeze", "(B) starve", "(C) hibernate"], correct: 2, explanation: "【變溫動物】hibernate (冬眠)。" },
    { id: 10659, subject: '英文', year: 106, text: "59. The extremely ________ 1918 flu pandemic... caused more than 40 million deaths worldwide...", options: ["(A) malignant", "(B) meek", "(C) unstable"], correct: 0, explanation: "【單字】malignant (致命的/惡性的)。" },
    { id: 10660, subject: '英文', year: 106, text: "60. Glycogen constitutes the body's ________ of glucose and is readily converted to energy...", options: ["(A) container", "(B) pool", "(C) product"], correct: 1, explanation: "【生理學】pool (儲存庫)。" },

        // --- 第二部分：克漏字文章作答 (61-80題) ---
    // 文章一：心理免疫學 (passageId: '106_p1')
    { 
        id: 10661, subject: '英文', year: 106, passageId: '106_p1',
        passage: "Scientists are now studying a new field of research called psychoimmunology. It is based (61) the idea that people who are depressed or have a lot of stress (62) more likely to become sick. Researchers have recently found a connection between diseases and stressful situations. To test this theory, psychoimmunologists are trying to find a link between the brain and the immune system. The immune system in our bodies fights the bacteria and viruses which cause disease. Therefore, (63) we are likely to get various diseases or not depends on how well our immune system works. Biologists used to think that the immune system was a separate, independent part of our bodies. Recently, (64), they have found that our brain can (65) our immune system.",
        text: "61. It is based (61) the idea...", 
        options: ["(A) with", "(B) in", "(C) on"], correct: 2, explanation: "be based on 為固定搭配。" 
    },
    { 
        id: 10662, subject: '英文', year: 106, passageId: '106_p1',
        passage: "Scientists are now studying a new field of research called psychoimmunology. It is based (61) the idea that people who are depressed or have a lot of stress (62) more likely to become sick. Researchers have recently found a connection between diseases and stressful situations. To test this theory, psychoimmunologists are trying to find a link between the brain and the immune system. The immune system in our bodies fights the bacteria and viruses which cause disease. Therefore, (63) we are likely to get various diseases or not depends on how well our immune system works. Biologists used to think that the immune system was a separate, independent part of our bodies. Recently, (64), they have found that our brain can (65) our immune system.",
        text: "62. ...people who are depressed or have a lot of stress (62) more likely to become sick.", options: ["(A) is", "(B) are", "(C) being"], correct: 1, explanation: "主詞為複數 people，動詞用 are。" 
    },
    { 
        id: 10663, subject: '英文', year: 106, passageId: '106_p1',
        passage: "Scientists are now studying a new field of research called psychoimmunology. It is based (61) the idea that people who are depressed or have a lot of stress (62) more likely to become sick. Researchers have recently found a connection between diseases and stressful situations. To test this theory, psychoimmunologists are trying to find a link between the brain and the immune system. The immune system in our bodies fights the bacteria and viruses which cause disease. Therefore, (63) we are likely to get various diseases or not depends on how well our immune system works. Biologists used to think that the immune system was a separate, independent part of our bodies. Recently, (64), they have found that our brain can (65) our immune system.",
        text: "63. Therefore, (63) we are likely to get various diseases or not depends on...", options: ["(A) why", "(B) whether", "(C) what"], correct: 1, explanation: "whether... or not (是否)。" 
    },
    { 
        id: 10664, subject: '英文', year: 106, passageId: '106_p1',
        passage: "Scientists are now studying a new field of research called psychoimmunology. It is based (61) the idea that people who are depressed or have a lot of stress (62) more likely to become sick. Researchers have recently found a connection between diseases and stressful situations. To test this theory, psychoimmunologists are trying to find a link between the brain and the immune system. The immune system in our bodies fights the bacteria and viruses which cause disease. Therefore, (63) we are likely to get various diseases or not depends on how well our immune system works. Biologists used to think that the immune system was a separate, independent part of our bodies. Recently, (64), they have found that our brain can (65) our immune system.",
        text: "64. Recently, (64), they have found that our brain can...", options: ["(A) however", "(B) consequently", "(C) therefore"], correct: 0, explanation: "語氣轉折用 however。" 
    },
    { 
        id: 10665, subject: '英文', year: 106, passageId: '106_p1',
        passage: "Scientists are now studying a new field of research called psychoimmunology. It is based (61) the idea that people who are depressed or have a lot of stress (62) more likely to become sick. Researchers have recently found a connection between diseases and stressful situations. To test this theory, psychoimmunologists are trying to find a link between the brain and the immune system. The immune system in our bodies fights the bacteria and viruses which cause disease. Therefore, (63) we are likely to get various diseases or not depends on how well our immune system works. Biologists used to think that the immune system was a separate, independent part of our bodies. Recently, (64), they have found that our brain can (65) our immune system.",
        text: "65. ...found that our brain can (65) our immune system.", options: ["(A) affect", "(B) blind", "(C) enlarge"], correct: 0, explanation: "大腦會『影響』免疫系統。" 
    },

    // 文章二：青蛙消失 (passageId: '106_p2')
    { 
        id: 10666, subject: '英文', year: 106, passageId: '106_p2',
        passage: "According to scientists, frogs are disappearing from the earth. Since 1997, the frog population has dropped (66) 2 percent every year. Some frogs species have completely (67). No one can be sure why this is happening, but a number of scientists are looking into it. One scientist, Professor J. Pechmann, is studying frogs at a pond. He believes there are several stresses on our planet that are killing off frogs. These problems (68) pollution, global warming and loss of the frog habitat. Pechmann warns the loss of frogs should be taken seriously. He says the decline of frogs may be a signal that something is going wrong with our environment. Even if we are not at (69), we may have to take (70) for the extinction of frog species.",
        text: "66. ...the frog population has dropped (66) 2 percent every year.",
        options: ["(A) at", "(B) for", "(C) by"], correct: 2, explanation: "drop by + 數字 (下降了多少比例)。"
    },
    { 
        id: 10667, subject: '英文', year: 106, passageId: '106_p2',
        passage: "According to scientists, frogs are disappearing from the earth. Since 1997, the frog population has dropped (66) 2 percent every year. Some frogs species have completely (67). No one can be sure why this is happening, but a number of scientists are looking into it. One scientist, Professor J. Pechmann, is studying frogs at a pond. He believes there are several stresses on our planet that are killing off frogs. These problems (68) pollution, global warming and loss of the frog habitat. Pechmann warns the loss of frogs should be taken seriously. He says the decline of frogs may be a signal that something is going wrong with our environment. Even if we are not at (69), we may have to take (70) for the extinction of frog species.",
        text: "67. Some frogs species have completely (67).", options: ["(A) vanished", "(B) finished", "(C) crashed"], correct: 0, explanation: "vanished (消失/滅絕)。" 
    },
    { 
        id: 10668, subject: '英文', year: 106, passageId: '106_p2',
        passage: "According to scientists, frogs are disappearing from the earth. Since 1997, the frog population has dropped (66) 2 percent every year. Some frogs species have completely (67). No one can be sure why this is happening, but a number of scientists are looking into it. One scientist, Professor J. Pechmann, is studying frogs at a pond. He believes there are several stresses on our planet that are killing off frogs. These problems (68) pollution, global warming and loss of the frog habitat. Pechmann warns the loss of frogs should be taken seriously. He says the decline of frogs may be a signal that something is going wrong with our environment. Even if we are not at (69), we may have to take (70) for the extinction of frog species.",
        text: "68. These problems (68) pollution, global warming...", options: ["(A) including", "(B) included", "(C) include"], correct: 2, explanation: "此句需要主動詞 include。" 
    },
    { 
        id: 10669, subject: '英文', year: 106, passageId: '106_p2',
        passage: "According to scientists, frogs are disappearing from the earth. Since 1997, the frog population has dropped (66) 2 percent every year. Some frogs species have completely (67). No one can be sure why this is happening, but a number of scientists are looking into it. One scientist, Professor J. Pechmann, is studying frogs at a pond. He believes there are several stresses on our planet that are killing off frogs. These problems (68) pollution, global warming and loss of the frog habitat. Pechmann warns the loss of frogs should be taken seriously. He says the decline of frogs may be a signal that something is going wrong with our environment. Even if we are not at (69), we may have to take (70) for the extinction of frog species.",
        text: "69. Even if we are not at (69)...", options: ["(A) peace", "(B) danger", "(C) risk"], correct: 2, explanation: "at risk (處於危險之中)。" 
    },
    { 
        id: 10670, subject: '英文', year: 106, passageId: '106_p2',
        passage: "According to scientists, frogs are disappearing from the earth. Since 1997, the frog population has dropped (66) 2 percent every year. Some frogs species have completely (67). No one can be sure why this is happening, but a number of scientists are looking into it. One scientist, Professor J. Pechmann, is studying frogs at a pond. He believes there are several stresses on our planet that are killing off frogs. These problems (68) pollution, global warming and loss of the frog habitat. Pechmann warns the loss of frogs should be taken seriously. He says the decline of frogs may be a signal that something is going wrong with our environment. Even if we are not at (69), we may have to take (70) for the extinction of frog species.",
        text: "70. ...we may have to take (70) for the extinction...", options: ["(A) blame", "(B) responsibility", "(C) stress"], correct: 1, explanation: "take responsibility for (為...負責)。" 
    },

    // 文章三：抗生素濫用 (passageId: '106_p3')
    {
        id: 10671, subject: '英文', year: 106, passageId: '106_p3',
        passage: "In modern hospitals, the most popular treatment for bacterial infection is antibiotics. (71), as the human population consumes more antibiotics, the infection-producing (72) become stronger and more resistant to the drugs. Another reason why antibiotics are losing their effectiveness is that doctors often prescribe them to patients too freely. Both doctors and patients prefer treatment providing fast relief, rather than (73) the body to battle the infection by itself. (74) unnecessary prescriptions are not the only source of antibiotics. They have been increasingly (75) on farms, where chickens and pigs are frequently fed antibiotics.",
        text: "71. (71), as the human population consumes more antibiotics...",
        options: ["(A) Consequently", "(B) Fortunately", "(C) Hopefully"], correct: 0, explanation: "Consequently (結果/因此)。"
    },
    { 
        id: 10672, subject: '英文', year: 106, passageId: '106_p3',
        passage: "In modern hospitals, the most popular treatment for bacterial infection is antibiotics. (71), as the human population consumes more antibiotics, the infection-producing (72) become stronger and more resistant to the drugs. Another reason why antibiotics are losing their effectiveness is that doctors often prescribe them to patients too freely. Both doctors and patients prefer treatment providing fast relief, rather than (73) the body to battle the infection by itself. (74) unnecessary prescriptions are not the only source of antibiotics. They have been increasingly (75) on farms, where chickens and pigs are frequently fed antibiotics.",
        text: "72. ...the infection-producing (72) become stronger...", options: ["(A) problems", "(B) treatments", "(C) bacteria"], correct: 2, explanation: "bacteria (細菌) 變強。" 
    },
    { 
        id: 10673, subject: '英文', year: 106, passageId: '106_p3',
        passage: "In modern hospitals, the most popular treatment for bacterial infection is antibiotics. (71), as the human population consumes more antibiotics, the infection-producing (72) become stronger and more resistant to the drugs. Another reason why antibiotics are losing their effectiveness is that doctors often prescribe them to patients too freely. Both doctors and patients prefer treatment providing fast relief, rather than (73) the body to battle the infection by itself. (74) unnecessary prescriptions are not the only source of antibiotics. They have been increasingly (75) on farms, where chickens and pigs are frequently fed antibiotics.",
        text: "73. ...rather than (73) the body to battle the infection by itself.", options: ["(A) allowing", "(B) allows", "(C) allowed"], correct: 0, explanation: "rather than 後接 V-ing 或原形。" 
    },
    { 
        id: 10674, subject: '英文', year: 106, passageId: '106_p3',
        passage: "In modern hospitals, the most popular treatment for bacterial infection is antibiotics. (71), as the human population consumes more antibiotics, the infection-producing (72) become stronger and more resistant to the drugs. Another reason why antibiotics are losing their effectiveness is that doctors often prescribe them to patients too freely. Both doctors and patients prefer treatment providing fast relief, rather than (73) the body to battle the infection by itself. (74) unnecessary prescriptions are not the only source of antibiotics. They have been increasingly (75) on farms, where chickens and pigs are frequently fed antibiotics.",
        text: "74. (74) unnecessary prescriptions are not the only source...", options: ["(A) If", "(B) But", "(C) What"], correct: 1, explanation: "語氣轉折使用 But。" 
    },
    { 
        id: 10675, subject: '英文', year: 106, passageId: '106_p3',
        passage: "In modern hospitals, the most popular treatment for bacterial infection is antibiotics. (71), as the human population consumes more antibiotics, the infection-producing (72) become stronger and more resistant to the drugs. Another reason why antibiotics are losing their effectiveness is that doctors often prescribe them to patients too freely. Both doctors and patients prefer treatment providing fast relief, rather than (73) the body to battle the infection by itself. (74) unnecessary prescriptions are not the only source of antibiotics. They have been increasingly (75) on farms, where chickens and pigs are frequently fed antibiotics.",
        text: "75. They have been increasingly (75) on farms...", options: ["(A) using", "(B) uses", "(C) used"], correct: 2, explanation: "被動語態：抗生素被使用 (used)。" 
    },

    // 文章四：肥胖問題 (passageId: '106_p4')
    {
        id: 10676, subject: '英文', year: 106, passageId: '106_p4',
        passage: "Being (76) overweight or obese is bad for both body and mind. Not only does it make a person feel tired and uncomfortable, it can wear down joints and put extra stress on other parts of the body. (77) a person is carrying extra weight, it's harder to keep up with friends and play sports. It is also associated (78) breathing problems, such as asthma and sleep apnea. There can be more seriously consequences (79). Obesity in young people can cause illnesses that once (80) to be problems only for adults, such as high blood pressure.",
        text: "76. Being (76) overweight or obese...",
        options: ["(A) extremely", "(B) partially", "(C) slightly"], correct: 0, explanation: "extremely (極度地)。"
    },
    { 
        id: 10677, subject: '英文', year: 106, passageId: '106_p4',
        passage: "Being (76) overweight or obese is bad for both body and mind. Not only does it make a person feel tired and uncomfortable, it can wear down joints and put extra stress on other parts of the body. (77) a person is carrying extra weight, it's harder to keep up with friends and play sports. It is also associated (78) breathing problems, such as asthma and sleep apnea. There can be more seriously consequences (79). Obesity in young people can cause illnesses that once (80) to be problems only for adults, such as high blood pressure.",
        text: "77. (77) a person is carrying extra weight, it's harder to...", options: ["(A) Whereas", "(B) When", "(C) Where"], correct: 1, explanation: "When (當...時)。" 
    },
    { 
        id: 10678, subject: '英文', year: 106, passageId: '106_p4',
        passage: "Being (76) overweight or obese is bad for both body and mind. Not only does it make a person feel tired and uncomfortable, it can wear down joints and put extra stress on other parts of the body. (77) a person is carrying extra weight, it's harder to keep up with friends and play sports. It is also associated (78) breathing problems, such as asthma and sleep apnea. There can be more seriously consequences (79). Obesity in young people can cause illnesses that once (80) to be problems only for adults, such as high blood pressure.",
        text: "78. It is also associated (78) breathing problems...", options: ["(A) to", "(B) with", "(C) as"], correct: 1, explanation: "be associated with (與...相關)。" 
    },
    { 
        id: 10679, subject: '英文', year: 106, passageId: '106_p4',
        passage: "Being (76) overweight or obese is bad for both body and mind. Not only does it make a person feel tired and uncomfortable, it can wear down joints and put extra stress on other parts of the body. (77) a person is carrying extra weight, it's harder to keep up with friends and play sports. It is also associated (78) breathing problems, such as asthma and sleep apnea. There can be more seriously consequences (79). Obesity in young people can cause illnesses that once (80) to be problems only for adults, such as high blood pressure.",
        text: "79. There can be more seriously consequences (79).", options: ["(A) as well", "(B) additionally", "(C) moreover"], correct: 0, explanation: "as well (也/同樣)。" 
    },
    { 
        id: 10680, subject: '英文', year: 106, passageId: '106_p4',
        passage: "Being (76) overweight or obese is bad for both body and mind. Not only does it make a person feel tired and uncomfortable, it can wear down joints and put extra stress on other parts of the body. (77) a person is carrying extra weight, it's harder to keep up with friends and play sports. It is also associated (78) breathing problems, such as asthma and sleep apnea. There can be more seriously consequences (79). Obesity in young people can cause illnesses that once (80) to be problems only for adults, such as high blood pressure.",
        text: "80. ...illnesses that once (80) to be problems only for adults...", options: ["(A) was known as", "(B) were thought", "(C) have considere"], correct: 1, explanation: "were thought to be (被認為是)。" 
    },

    // --- 107年度 第一部分：單句文法與詞彙 (1-60題) ---
  { id: 10701, subject: '英文', year: 107, text: "1. Doggie day camps are great for improving your dog's socialization as your pet will ________ with a variety of other dogs.", options: ["(A) associate", "(B) mingle", "(C) puzzle", "(D) manipulate"], correct: 1, explanation: "mingle with 意為與...社交、混合。" },
  { id: 10702, subject: '英文', year: 107, text: "2. Besides proving energy for basic ________ functions and movement, food can help support the body's ability to maintain a strong muscular-skeletal structure.", options: ["(A) metabolic", "(B) metabolism", "(C) metabolize", "(D) metabolite"], correct: 0, explanation: "metabolic 為形容詞，修飾 functions（代謝功能）。" },
  { id: 10703, subject: '英文', year: 107, text: "3. By living in a large ________, birds are able to attack the predator with a stronger force compared to if the bird was on its own.", options: ["(A) flock", "(B) block", "(C) clock", "(D) mock"], correct: 0, explanation: "a flock of birds 指一群鳥。" },
  { id: 10704, subject: '英文', year: 107, text: "4. The flea's bite can cause itching... and leads to hair-loss, inflammation and secondary skin ________.", options: ["(A) reflections", "(B) infections", "(C) extensions", "(D) inflections"], correct: 1, explanation: "skin infections 意為皮膚感染。" },
  { id: 10705, subject: '英文', year: 107, text: "5. Not only the children but also their dog ________ eating hamburgers.", options: ["(A) like", "(B) liking", "(C) to like", "(D) likes"], correct: 3, explanation: "Not only A but also B 句型中，動詞須隨靠近的主詞 (their dog) 變化，故用單數 likes。" },
  { id: 10706, subject: '英文', year: 107, text: "6. A great ________ is passionate about animals and committed to providing them the best care.", options: ["(A) victor", "(B) veteran", "(C) veterinarian", "(D) butcher"], correct: 2, explanation: "veterinarian 為獸醫。" },
  { id: 10707, subject: '英文', year: 107, text: "7. All colleges of veterinary medicine require ________ of numerous science courses, complete with labs.", options: ["(A) completion", "(B) indication", "(C) operation", "(D) conduction"], correct: 0, explanation: "completion 意為完成、結束。" },
  { id: 10708, subject: '英文', year: 107, text: "8. Vets need to be ________ but they also must be able to perform difficult tasks, such as euthanizing an animal.", options: ["(A) compassionate", "(B) confessional", "(C) professional", "(D) depressible"], correct: 0, explanation: "compassionate 意為富於同情心的。" },
  { id: 10709, subject: '英文', year: 107, text: "9. When I was growing up, the family dog wasn't allowed in the house, and the cats ________ in and out at will.", options: ["(A) fled", "(B) countered", "(C) wondered", "(D) wandered"], correct: 3, explanation: "wandered 意為漫步、遊蕩。" },
  { id: 10710, subject: '英文', year: 107, text: "10. Over the past several decades, society's attitudes have changed... and we keep them in close ________ as part of the family.", options: ["(A) maxim", "(B) comrade", "(C) convention", "(D) proximity"], correct: 3, explanation: "in close proximity 意為距離很近。" },
  { id: 10711, subject: '英文', year: 107, text: "11. Roundworms may cause no ________ in some people, but cause eye damage in others.", options: ["(A) symptoms", "(B) sympathy", "(C) symphony", "(D) epiphany"], correct: 0, explanation: "symptoms 意為症狀。" },
  { id: 10712, subject: '英文', year: 107, text: "12. Your pet can benefit greatly from regular wellness examinations or ________.", options: ["(A) pushups", "(B) backups", "(C) checkout", "(D) checkups"], correct: 3, explanation: "checkups 意為健康檢查。" },
  { id: 10713, subject: '英文', year: 107, text: "13. People often say they don't see their dog's true personality until several weeks after ________.", options: ["(A) adaptor", "(B) adoption", "(C) adaption", "(D) recursion"], correct: 1, explanation: "adoption 意為領養。" },
  { id: 10714, subject: '英文', year: 107, text: "14. Just as annual physical exams are recommended for humans, they are recommended for pets ________.", options: ["(A) as follows", "(B) at all", "(C) as well", "(D) at will"], correct: 2, explanation: "as well 意為「也」。" },
  { id: 10715, subject: '英文', year: 107, text: "15. ________ are the best way to protect our pets from lots of nasty diseases.", options: ["(A) Vacation", "(B) Vaccinations", "(C) Vaccinate", "(D) Vacuum"], correct: 1, explanation: "Vaccinations 意為疫苗接種。" },
  { id: 10716, subject: '英文', year: 107, text: "16. ________ is the giving of an anthelmintic drug to a human or animal to rid them of helminths parasites.", options: ["(A) Devoicing", "(B) Deleting", "(C) Drawing", "(D) Deworming"], correct: 3, explanation: "Deworming 意為驅蟲。" },
  { id: 10717, subject: '英文', year: 107, text: "17. Nylon varieties are known for being the best option if you're looking for a durable and functional dog ________.", options: ["(A) collapse", "(B) color", "(C) collar", "(D) collard"], correct: 2, explanation: "collar 意為項圈。" },
  { id: 10718, subject: '英文', year: 107, text: "18. A ________ can serve many purposes, including keeping an animal in captivity, capturing, and being used for display.", options: ["(A) cage", "(B) net", "(C) coverage", "(D) tin"], correct: 0, explanation: "cage 意為籠子。" },
  { id: 10719, subject: '英文', year: 107, text: "19. The main symptoms of ________ are increased difficulty and straining when passing stools.", options: ["(A) constipation", "(B) conversation", "(C) competition", "(D) constitutive"], correct: 0, explanation: "constipation 意為便秘。" },
  { id: 10720, subject: '英文', year: 107, text: "20. No instrument has had such widespread application ________ the clinical thermometer.", options: ["(A) by", "(B) on", "(C) with", "(D) as"], correct: 3, explanation: "as...as 結構，意為「如同...一樣廣泛」。" },
  { id: 10724, subject: '英文', year: 107, text: "24. Most birds are ________ which means they are most active during the day but typically rest at night.", options: ["(A) diurnal", "(B) bilingual", "(C) amphibious", "(D) biannual"], correct: 0, explanation: "diurnal 意為晝行性的。" },
  { id: 10750, subject: '英文', year: 107, text: "50. ________ the disease can be transmitted through air is still uncertain.", options: ["(A) Whoever", "(B) Which", "(C) Weather", "(D) If"], correct: 3, explanation: "If 引導名詞子句作為主詞，意為「是否」。" },

  // --- 克漏字 (61-80) ---
  { 
    id: 10761, subject: '英文', year: 107, 
    passage: "The oceans are dying. But it's not (61) late to help. Plastic marine litter affects at least 267 species worldwide...",
    text: "61. But it's not (61) late to help.", 
    options: ["(A) so", "(B) many", "(C) too", "(D) enough"], correct: 2, explanation: "not too late 意為還不算太晚。" 
  },
  { 
    id: 10762, subject: '英文', year: 107, 
    passage: "Plastic marine litter affects at least 267 species worldwide (62) sea turtles, whales, dolphins and marine birds.",
    text: "62. ...worldwide (62) sea turtles...", 
    options: ["(A) includes", "(B) including", "(C) include", "(D) included"], correct: 1, explanation: "including 作介系詞，意為包括。" 
  },
  { 
    id: 10763, subject: '英文', year: 107, 
    passage: "A huge amount of plastic litter ends up in our oceans every year. (63), the ocean currents have formed five massive whirlpools...",
    text: "63. (63), the ocean currents have formed...", 
    options: ["(A) As a result", "(B) However", "(C) By the way", "(D) On the one hand"], correct: 0, explanation: "As a result 意為因此、結果。" 
  },
  { 
    id: 10766, subject: '英文', year: 107, 
    passage: "Dogs... always look (66), and... you swear they can tell (67) having a bad day... (68) According to Mental Floss...",
    text: "66. ...they always look (66)...", 
    options: ["(A) enthuse", "(B) enthusiast", "(C) enthusiasm", "(D) enthusiastic"], correct: 3, explanation: "look 為連綴動詞，後接形容詞 enthusiastic。" 
  },
  { 
    id: 10768, subject: '英文', year: 107, 
    passage: "(68) Mental Floss, there are several explanations...",
    text: "68. (68) Mental Floss...", 
    options: ["(A) In support of", "(B) On account of", "(C) According to", "(D) In consequence of"], correct: 2, explanation: "According to 意為根據。" 
  },
  { 
    id: 10773, subject: '英文', year: 107, 
    passage: "Here, wood fuels (73) between 50 and 90 percent of the fuel used.",
    text: "73. ...wood fuels (73) between 50 and 90 percent...", 
    options: ["(A) stand for", "(B) in support of", "(C) account for", "(D) account to"], correct: 2, explanation: "account for 意為佔了（多少比例）。" 
  },
  { 
    id: 10777, subject: '英文', year: 107, 
    passage: "The interest in the adequacy of commercially available pet foods (77) growing worldwide.",
    text: "77. ...pet foods (77) growing worldwide.", 
    options: ["(A) are", "(B) has", "(C) have been", "(D) has been"], correct: 3, explanation: "單數主詞 interest 搭配 has been growing（現在完成進行式）。" 
  },
  { 
    id: 10780, subject: '英文', year: 107, 
    passage: "...can modify the intestinal microflora by (80) commensal bacteria growth.",
    text: "80. ...by (80) commensal bacteria growth.", 
    options: ["(A) promote", "(B) promoting", "(C) promotion", "(D) promoted"], correct: 1, explanation: "介系詞 by 之後動詞須加 ing 變動名詞。" 
  }
]);

                const availableYears = computed(() => {
                    const years = allQuestions.value
                        .filter(q => q.subject === activeSubject.value)
                        .map(q => q.year);
                    return [...new Set(years)].sort((a, b) => b - a);
                });

                const getQuestionCount = (year) => allQuestions.value.filter(q => q.subject === activeSubject.value && q.year === year).length;

                const filteredQuestions = computed(() => 
                    allQuestions.value.filter(q => q.subject === activeSubject.value && q.year === activeYear.value)
                );
                
                const currentQuestion = computed(() => filteredQuestions.value[currentQuestionIndex.value] || {});

                const totalProgress = computed(() => {
                    const total = subjects.value.reduce((acc, s) => acc + s.progress, 0);
                    return Math.round(total / subjects.value.length);
                });

                const selectSubject = (name) => { activeSubject.value = name; activeYear.value = null; };
                const selectYear = (year) => {
                    activeYear.value = year;
                    currentQuestionIndex.value = 0;
                    selectedOption.value = null;
                    showFeedback.value = false;
                    quizFinished.value = false;
                    correctCount.value = 0;
                    startTimer();
                };

                // --- NEW: History Functions ---
                const saveHistory = () => {
                    const key = `${activeSubject.value}_${activeYear.value}`;
                    const score = Math.round((correctCount.value / filteredQuestions.value.length) * 100);
                    const now = new Date();
                    quizHistory.value[key] = {
                        score: score,
                        date: `${now.getMonth() + 1}/${now.getDate()} ${now.getHours()}:${now.getMinutes().toString().padStart(2,'0')}`
                    };
                };

                const getHistory = (subject, year) => {
                    return quizHistory.value[`${subject}_${year}`] || null;
                };

                const goBack = () => {
                    if (activeYear.value) activeYear.value = null;
                    else activeSubject.value = null;
                };

                const startTimer = () => {
                    if (timerInterval) clearInterval(timerInterval);
                    timer.value = 600;
                    timerInterval = setInterval(() => { if (timer.value > 0) timer.value--; }, 1000);
                };

                const jumpToQuestion = (idx) => {
                    currentQuestionIndex.value = idx;
                    selectedOption.value = null;
                    showFeedback.value = false;
                    showNavigator.value = false;
                };

                const selectOption = (idx) => {
                    selectedOption.value = idx;
                    showFeedback.value = true;
                    if (idx === currentQuestion.value.correct) {
                        correctCount.value++;
                        const sub = subjects.value.find(s => s.name === activeSubject.value);
                        if (sub) sub.progress = Math.min(100, sub.progress + 0.5);
                    } else {
                        if (!errorLog.value.some(e => e.id === currentQuestion.value.id)) {
                            errorLog.value.push(currentQuestion.value);
                        }
                    }
                };

                const getOptionClass = (idx) => {
                    if (!showFeedback.value) return selectedOption.value === idx ? 'border-[#C3B091] bg-[#F9F6F0]' : 'border-gray-100';
                    if (idx === currentQuestion.value.correct) return 'border-emerald-500 bg-emerald-50';
                    if (selectedOption.value === idx) return 'border-red-500 bg-red-50';
                    return 'border-gray-50 opacity-50';
                };

                const nextQuestion = () => {
                    if (currentQuestionIndex.value + 1 < filteredQuestions.value.length) {
                        jumpToQuestion(currentQuestionIndex.value + 1);
                    } else { 
                        quizFinished.value = true; 
                        clearInterval(timerInterval); 
                        saveHistory(); // Save score on finish
                    }
                };

                const toggleFavorite = (q) => {
                    const idx = favorites.value.findIndex(f => f.id === q.id);
                    if (idx > -1) favorites.value.splice(idx, 1);
                    else favorites.value.push(q);
                };

                const isFavorited = (id) => favorites.value.some(f => f.id === id);
                const getFilteredItems = (list) => list.filter(item => item.subject === filterSub.value);
                const formatTime = (s) => `${Math.floor(s/60)}:${(s%60).toString().padStart(2,'0')}`;
                const resetAllData = () => { if(confirm("確定重置紀錄？")) { localStorage.clear(); location.reload(); } };

                return { 
                    currentTab, activeSubject, activeYear, subjects, totalProgress, filterSub, showNavigator, 
                    favorites, errorLog, filteredQuestions, currentQuestion, currentQuestionIndex,
                    selectedOption, showFeedback, quizFinished, timer, correctCount, availableYears,
                    selectSubject, selectYear, goBack, jumpToQuestion, selectOption, getOptionClass, nextQuestion, 
                    toggleFavorite, isFavorited, getFilteredItems, formatTime, resetAllData, getQuestionCount,
                    getHistory
                };
            }
        }).mount('#app');
    </script>
</body>
</html>
