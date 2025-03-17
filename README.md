# 📓 Lifting Ledger  
**Your Ultimate Workout Logging Companion**  

📌 [Lifting Tracker - source code](https://github.com/Alec-Dipasquale/Lifting-Tracker-KMP)

## 🎯 Overview  
**Lifting Ledger** is an advanced workout tracking application built with **Kotlin Multiplatform (KMM)** and **Jetpack Compose**. It provides a **structured logging system**, powerful **search and filtering mechanisms**, and an **optimized UI for real-time data interaction**.  

The app leverages **Room Database with complex queries**, **state-driven architecture**, and **real-time search indexing** to deliver a smooth and efficient experience for users tracking their workouts.

---

## 🚀 Features  

### 🏋️ **Workout Session Management**  
- **Dynamic workout creation** with **real-time session tracking**.  
- **Relational database structure**: `Workout → Exercises → Sets`.  
- Efficient **data persistence** while minimizing unnecessary disk writes.  

<img src="lifting_ledger_start_workout.gif" width="400px">

🔹 **Optimized with:**  
- **Event-driven ViewModel architecture** using `StateFlow` for real-time UI updates.  
- **LazyColumn** for rendering exercises dynamically without performance overhead.  
- **In-memory session management** to prevent redundant database writes.  

---

### 📅 **Workout History & Calendar Integration**  
- **View past workouts** with an interactive calendar.  
- **Modify workout details dynamically** without full recompositions.  
- **Third-party calendar integration** for efficient date-based filtering.  

<img src="lifting_ledger_finish_and_calendar.gif" width="400px">

🔹 **Performance Highlights:**  
- Uses **LazyColumn with stable keys** for seamless UI performance.  
- Implements **efficient data retrieval** with indexed Room queries.  
- Leverages [`kizitonwose/calendar`](https://github.com/kizitonwose/CalendarView) for optimized calendar UI rendering.

---

### 🔍 **Advanced Search Functionality**  
- **Full-text search indexing (FTS)** for instant lookup.  
- **Multi-muscle filtering** with **AND/OR** operators.  
- **Real-time UI updates** without blocking the main thread.  

<img src="lifting_ledger_search_function_v2.gif" width="400px">

🔹 **Optimized Search Implementation:**  
```sql
SELECT DISTINCT * FROM exercise_details
WHERE LOWER(name) LIKE '%' || LOWER(:search) || '%'
AND (
    (:muscle1 IS NULL OR primaryMuscles LIKE '%' || :muscle1 || '%')
    OR (:muscle2 IS NULL OR primaryMuscles LIKE '%' || :muscle2 || '%')
    OR (:muscle3 IS NULL OR primaryMuscles LIKE '%' || :muscle3 || '%')
)
```
- **Leverages Room’s Full-Text Search (FTS)** for instant results.  
- **Utilizes indexed columns** to enhance performance on extensive datasets.  
- **Employs Flow-based state management** to minimize unnecessary UI recompositions.  

---

### 📌 **Exercise Detail View**  
- **Comprehensive breakdown** of each exercise.  
- **Interactive image carousel** providing visual guidance.  
- **Insights into muscles targeted, equipment used, and mechanics** to assist in planning.  

![Exercise Detail View](lifting_ledger_exercise_info_screen.png)

---

## 🛠 **Tech Stack**  
- **Kotlin Multiplatform (KMM)** – Ensures cross-platform compatibility.  
- **Jetpack Compose** – Modern, declarative UI framework.  
- **Room Database with FTS** – Facilitates optimized full-text search and complex relational queries.  
- **Coroutines & Flow** – Supports asynchronous processing for real-time updates.  
- **MVVM Architecture** – Promotes a clean, scalable design.  
- **Material Design 3** – Provides a polished and consistent UI.  
- **Third-party Libraries:**  
  - [`kizitonwose/calendar`](https://github.com/kizitonwose/CalendarView) – Integrates an efficient calendar UI.  

---

## ⚙️ **Technical Deep Dive**  

### **1️⃣ Room Query Optimization & Full-Text Search (FTS)**  
**🔹 Challenge:** Implementing **real-time search** with **complex filtering** while maintaining performance.  

**✅ Solution:**  
- **Full-Text Search (FTS)** ensures **low-latency queries** across large datasets.  
- **Indexed search fields** enhance performance.  
- **Parameterized Queries** optimize SQL execution paths.  

**Example: Efficient Full-Text Search Implementation**
```kotlin
@Query("SELECT * FROM exercise_details WHERE LOWER(name) LIKE '%' || LOWER(:search) || '%'")
suspend fun searchExercises(search: String): List<ExerciseDetails>
```
- **Case-insensitive search** avoids redundant post-processing.  
- **Indexed lookups** minimize query execution time.  

---

### **2️⃣ Multi-Muscle Filtering with Dynamic Query Expansion**  
**🔹 Challenge:** Supporting **complex filtering** based on **multiple muscle groups**.  

**✅ Solution:**  
- Implemented **dynamic SQL generation** for **flexible AND/OR filtering**.  
- Utilized **Room’s JSON support** to store and retrieve structured data.  
- Ensured **scalable query performance** as the dataset grows.  

**Example: Multi-Muscle Filtering Query**
```sql
SELECT DISTINCT * FROM exercise_details
WHERE LOWER(name) LIKE '%' || LOWER(:search) || '%'
AND (
    (:muscle1 IS NULL OR primaryMuscles LIKE '%' || :muscle1 || '%')
    OR (:muscle2 IS NULL OR primaryMuscles LIKE '%' || :muscle2 || '%')
    OR (:muscle3 IS NULL OR primaryMuscles LIKE '%' || :muscle3 || '%')
)
```
- **Supports up to 7 muscle groups dynamically.**  
- **Ensures efficient filtering** while maintaining search responsiveness.  

---

### **3️⃣ UI Performance Optimizations in Jetpack Compose**  
**🔹 Challenge:** Ensuring **smooth UI rendering** while handling **large exercise lists**.  

**✅ Solution:**  
- **LazyColumn with Stable Keys** prevents unnecessary recompositions.  
- **StateFlow-based UI updates** avoid UI freezes.  
- **Asynchronous data loading** ensures smooth scrolling performance.  

**Example: Optimized LazyColumn Usage**
```kotlin
LazyColumn {
    items(exerciseList, key = { it.id }) { exercise ->
        ExerciseItem(exercise)
    }
}
```
- **Prevents list state loss** during scrolling.  
- **Minimizes UI redraws** for a responsive experience.  

---

## 📲 **Installation**  

### 1️⃣ Clone the Repository  
```bash
git clone https://github.com/Alec-Dipasquale/Lifting-Tracker-KMP.git
cd Lifting-Tracker-KMP
```
### 2️⃣ Open in Android Studio  
- **Ensure Android Studio Flamingo+ is installed.**  
- Open the project and **sync Gradle dependencies**.

### 3️⃣ Run the App  
- Select an emulator or physical device.  
- Click **Run ▶** to launch **Lifting Ledger**!

---

## 🔥 **Future Enhancements**  
- **🔄 Cloud Sync Support** – Implement Firebase for cross-device workout tracking.  
- **📊 Progress Analytics** – Add real-time data visualization for tracking performance.  
- **📝 Enhanced Workout Editing** – Allow users to modify/delete exercises & sets.  
- **📥 Room Paging** – Implement lazy loading for workout history.  

---

## 📜 **License**  
MIT License – Free to use, modify, and distribute.  
