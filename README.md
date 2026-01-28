<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Speed Quiz: English Mastery</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Pretendard:wght@400;600;700&display=swap');
        body { font-family: 'Pretendard', sans-serif; background-color: #f8fafc; }
        .card-flip { perspective: 1000px; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useMemo } = React;

        // --- 데이터셋 구성 (사용자 제공 데이터 기반 샘플링) ---
        // 실제 데이터가 방대하므로 구조를 잡고 대표 데이터 일부를 넣었습니다. 
        // 이 부분을 확장하여 150문제를 모두 채울 수 있습니다.
        const RAW_DATA = {
            conversation: [
                { id: 1, day: 1, kr: "재택근무는 저랑 안 맞아요.", en: "Working from home isn’t for me." },
                { id: 2, day: 1, kr: "하루빨리 새 집으로 이사 가고 싶어요.", en: "I can’t wait to move into the new house." },
                { id: 50, day: 4, kr: "여기서는 원래 그래요.", en: "That’s just how things work here." },
                // ... 데이터 추가
            ],
            phrasal: [
                { id: 101, day: 1, kr: "자, 이 숫자들을 더해 보자.", en: "Let’s add up these numbers now.", verb: "add up" },
                { id: 102, day: 1, kr: "말이 되다(뭔가 안 맞다)", en: "Something doesn’t add up.", verb: "add up" },
                // ... 데이터 추가
            ],
            basic: [
                { id: 201, day: 1, kr: "반려동물 키우시나요?", en: "Do you have any pets?" },
                { id: 250, day: 4, kr: "오믈렛 팬은 당신이 가져요.", en: "You can keep the omelet pan." },
                // ... 데이터 추가
            ]
        };

        const App = () => {
            const [currentDay, setCurrentDay] = useState(1);
            const [quizMode, setQuizMode] = useState(false);
            const [quizList, setQuizList] = useState([]);
            const [currentIndex, setCurrentIndex] = useState(0);
            const [showAnswer, setShowAnswer] = useState(false);
            const [bookmarks, setBookmarks] = useState(new Set());
            const [difficultItems, setDifficultItems] = useState([]);
            const [isFinished, setIsFinished] = useState(false);

            // 퀴즈 생성 로직
            const startQuiz = (day) => {
                // 각 카테고리에서 해당 Day 범위 혹은 랜덤 5개 추출
                const getItems = (arr) => [...arr].sort(() => 0.5 - Math.random()).slice(0, 5);
                
                const sessionQuiz = [
                    ...getItems(RAW_DATA.conversation),
                    ...getItems(RAW_DATA.phrasal),
                    ...getItems(RAW_DATA.basic)
                ].sort(() => 0.5 - Math.random());

                setQuizList(sessionQuiz);
                setCurrentIndex(0);
                setShowAnswer(false);
                setQuizMode(true);
                setIsFinished(false);
                setDifficultItems([]);
            };

            const toggleBookmark = (id) => {
                const newBookmarks = new Set(bookmarks);
                if (newBookmarks.has(id)) newBookmarks.delete(id);
                else newBookmarks.add(id);
                setBookmarks(newBookmarks);
            };

            const handleNext = (wasHard) => {
                if (wasHard) {
                    setDifficultItems([...difficultItems, quizList[currentIndex]]);
                }
                
                if (currentIndex < quizList.length - 1) {
                    setCurrentIndex(currentIndex + 1);
                    setShowAnswer(false);
                } else {
                    setIsFinished(true);
                }
            };

            return (
                <div className="max-w-md mx-auto min-h-screen p-4 flex flex-col">
                    <header className="py-6 text-center">
                        <h1 className="text-2xl font-bold text-blue-600">Speed English 10 Days</h1>
                        <p className="text-gray-500 text-sm mt-1">회화 · 구동사 · 기본동사 정복</p>
                    </header>

                    {!quizMode ? (
                        <div className="grid grid-cols-2 gap-3 mt-4">
                            {[...Array(10)].map((_, i) => (
                                <button 
                                    key={i}
                                    onClick={() => startQuiz(i + 1)}
                                    className="bg-white border-2 border-blue-100 hover:border-blue-500 p-4 rounded-2xl shadow-sm transition-all text-left group"
                                >
                                    <span className="block text-xs font-bold text-blue-400 uppercase">Day</span>
                                    <span className="text-xl font-bold text-gray-800">{String(i + 1).padStart(2, '0')}</span>
                                    <i className="fa-solid fa-circle-play float-right text-blue-200 group-hover:text-blue-500 transition-colors mt-1"></i>
                                </button>
                            ))}
                        </div>
                    ) : isFinished ? (
                        <div className="bg-white rounded-3xl p-6 shadow-xl border border-gray-100 animate-fadeIn">
                            <h2 className="text-xl font-bold mb-4 text-center">오늘의 학습 완료! 🎉</h2>
                            <p className="text-gray-600 text-center mb-6">어려웠던 문장들을 복습해보세요.</p>
                            
                            <div className="space-y-4 max-h-[400px] overflow-y-auto pr-2 text-sm">
                                {difficultItems.length > 0 ? difficultItems.map((item, idx) => (
                                    <div key={idx} className="p-3 bg-red-50 rounded-xl border border-red-100">
                                        <div className="font-bold text-red-600 mb-1">{item.en}</div>
                                        <div className="text-gray-600">{item.kr}</div>
                                    </div>
                                )) : <div className="text-center text-gray-400">완벽합니다! 체크한 문장이 없어요.</div>}
                            </div>

                            <button 
                                onClick={() => setQuizMode(false)}
                                className="w-full mt-8 bg-blue-600 text-white py-4 rounded-2xl font-bold shadow-lg shadow-blue-200"
                            >
                                메인으로 돌아가기
                            </button>
                        </div>
                    ) : (
                        <div className="flex-1 flex flex-col">
                            <div className="flex justify-between items-center mb-4">
                                <span className="text-sm font-bold text-gray-400">{currentIndex + 1} / 15</span>
                                <button onClick={() => setQuizMode(false)} className="text-gray-400 hover:text-gray-600">
                                    <i className="fa-solid fa-xmark text-xl"></i>
                                </button>
                            </div>

                            <div 
                                className="flex-1 bg-white rounded-3xl shadow-xl p-8 flex flex-col items-center justify-center text-center relative cursor-pointer border border-gray-50"
                                onClick={() => setShowAnswer(!showAnswer)}
                            >
                                <button 
                                    className={`absolute top-6 right-6 text-2xl transition-colors ${bookmarks.has(quizList[currentIndex]?.id) ? 'text-yellow-400' : 'text-gray-200'}`}
                                    onClick={(e) => { e.stopPropagation(); toggleBookmark(quizList[currentIndex]?.id); }}
                                >
                                    <i className="fa-solid fa-star"></i>
                                </button>

                                <div className="text-blue-500 font-bold text-xs mb-4 uppercase tracking-widest">Can you translate?</div>
                                <div className="text-2xl font-bold text-gray-800 leading-relaxed mb-8">
                                    {quizList[currentIndex]?.kr}
                                </div>

                                {showAnswer && (
                                    <div className="animate-bounceIn">
                                        <div className="h-px w-12 bg-gray-100 mx-auto mb-6"></div>
                                        <div className="text-2xl font-bold text-blue-600 px-4 leading-tight">
                                            {quizList[currentIndex]?.en}
                                        </div>
                                    </div>
                                )}
                                
                                {!showAnswer && <div className="text-gray-300 text-sm mt-4 italic">터치해서 정답 확인</div>}
                            </div>

                            <div className="grid grid-cols-2 gap-4 mt-8">
                                <button 
                                    onClick={() => handleNext(true)}
                                    className="bg-gray-100 text-gray-600 py-4 rounded-2xl font-bold hover:bg-red-50 hover:text-red-500 transition-colors"
                                >
                                    어려워요 ⭐
                                </button>
                                <button 
                                    onClick={() => handleNext(false)}
                                    className="bg-blue-600 text-white py-4 rounded-2xl font-bold shadow-lg shadow-blue-200"
                                >
                                    쉬워요!
                                </button>
                            </div>
                        </div>
                    )}
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
