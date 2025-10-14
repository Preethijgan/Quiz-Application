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
