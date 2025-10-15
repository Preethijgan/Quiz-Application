## Problem:

Develop a Quiz App where:

Users can answer multiple-choice questions

The app tracks score

Users can reset the quiz

Timer counts how long the quiz took

## Use:

useReducer → to manage quiz state (NEXT_QUESTION, ANSWER_SELECTED, RESET)

useRef → to handle the timer interval ID (start, stop without re-renders)

useMemo → to calculate the final score only when the quiz is finished (avoid recalculating every render)

useCallback → to memoize functions like handleAnswerSelect and handleResetQuiz

## Hints:

useReducer keeps question index and score

useRef manages setInterval and clearInterval

useMemo computes final score when quizOver changes

useCallback prevents re-rendering of answer buttons

## Code Implementation:

QuizApp.jsx
```
import React, { useState, useEffect, useMemo, useCallback, useRef, useReducer } from "react";
import "../styles/style.css";

const questions = [
  { question: "What is the capital of France?", options: ["Paris", "London", "Berlin", "Madrid"], answer: "Paris" },
  { question: "2 + 2 * 2 = ?", options: ["6", "8", "4", "2"], answer: "6" },
  { question: "Which language runs in a browser?", options: ["Java", "C", "Python", "JavaScript"], answer: "JavaScript" }
];

function quizReducer(state, action) {
  switch (action.type) {
    case "NEXT": return { ...state, currentQuestion: state.currentQuestion + 1 };
    case "RESET": return { currentQuestion: 0, score: 0 };
    case "SCORE": return { ...state, score: state.score + 1 };
    default: return state;
  }
}

function QuizApp() {
  const [state, dispatch] = useReducer(quizReducer, { currentQuestion: 0, score: 0 });
  const [time, setTime] = useState(0);
  const timerRef = useRef(null);

  useEffect(() => {
    timerRef.current = setInterval(() => setTime((t) => t + 1), 1000);
    return () => clearInterval(timerRef.current);
  }, []);

  const currentQ = useMemo(() => questions[state.currentQuestion], [state.currentQuestion]);

  const selectAnswer = useCallback(
    (option) => {
      if (option === currentQ.answer) dispatch({ type: "SCORE" });
      dispatch({ type: "NEXT" });
    },
    [currentQ]
  );

  const resetQuiz = useCallback(() => {
    dispatch({ type: "RESET" });
    setTime(0);
  }, []);

  if (!currentQ) {
    clearInterval(timerRef.current);
    return (
      <div className="quiz-container">
        <h1>Quiz Finished!</h1>
        <p>Score: {state.score}</p>
        <p>Time: {time}s</p>
        <button onClick={resetQuiz}>Reset Quiz</button>
      </div>
    );
  }

  return (
    <div className="quiz-container">
      <h1>Quiz App</h1>
      <p>Time: {time}s</p>
      <h2>{currentQ.question}</h2>
      <div>
        {currentQ.options.map((option) => (
          <button key={option} onClick={() => selectAnswer(option)}>
            {option}
          </button>
        ))}
      </div>
      <p>Score: {state.score}</p>
      <button onClick={resetQuiz}>Reset Quiz</button>
    </div>
  );
}

export default QuizApp;


```

APP.JSX
```
import React from "react";
import QuizApp from "./components/QuizApp";

function App() {
  return <QuizApp />;
}

export default App;

```
## OUTPUT:
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/e901efb3-8f53-463a-9ae2-e36833af83ed" />

