<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Humsafar Meals Tracker</title>
    <!-- Tailwinds CSS Engine Loading -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- React Core & Babel Production Engines Loading -->
    <script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js" crossorigin></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body class="bg-gray-100">

    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        const mealPlanData = {
          Monday: {
            focus: "Fiber & Protein Boost",
            breakfast: { name: "Masala Omelette & Toast", ingredients: ["Eggs", "Spinach", "Tomatoes", "Onions", "Multi-grain Bread"] },
            lunch: { name: "Dal Tadka & Salad with Rotis", ingredients: ["Yellow Lentils", "Cucumber", "Tomatoes", "Onions", "Atta (Flour)"] },
            dinner: { name: "Home-style Chicken Curry & Veggies", ingredients: ["Chicken Breast", "Broccoli", "Green Beans", "Onions", "Ginger-Garlic Paste", "Spices"] },
            tip: "The 50% Salad Rule: Fill half your plate with salad or veggies before adding the main curry and carbs."
          },
          Tuesday: {
            focus: "Sustained Energy & Metabolism",
            breakfast: { name: "Vegetable Oats Upma", ingredients: ["Oats", "Carrots", "Peas", "Peanuts"] },
            lunch: { name: "Leftover Chicken Curry", ingredients: ["Chicken Breast", "Broccoli", "Green Beans"] },
            dinner: { name: "Spicy Egg Curry & Brown Rice", ingredients: ["Eggs", "Brown Rice", "Tomatoes", "Onions", "Spices"] },
            tip: "Switching to brown rice or quinoa provides a lower glycemic index, preventing afternoon energy crashes."
          },
          Wednesday: {
            focus: "Lean Mass Maintenance",
            breakfast: { name: "Moong Dal Chilla", ingredients: ["Moong Dal", "Low-fat Paneer", "Mint Chutney"] },
            lunch: { name: "Leftover Egg Curry & Curd", ingredients: ["Eggs", "Curd (Yogurt)", "Atta (Flour)"] },
            dinner: { name: "Jeera Aloo & Black Chana Masala", ingredients: ["Potatoes", "Black Chana (Chickpeas)", "Onions", "Tomatoes", "Besan (Gram Flour)"] },
            tip: "Missi Roti (made with chickpea flour) adds a powerful punch of plant-based protein to your dinner."
          },
          Thursday: {
            focus: "Gut Health & Muscle Repair",
            breakfast: { name: "Greek Yogurt Parfait", ingredients: ["Greek Yogurt", "Pomegranate Seeds", "Walnuts", "Chia Seeds"] },
            lunch: { name: "Leftover Black Chana Masala & Salad", ingredients: ["Black Chana (Chickpeas)", "Cucumber", "Tomatoes", "Mixed Greens"] },
            dinner: { name: "Tandoori Chicken Breast & Peppers", ingredients: ["Chicken Breast", "Bell Peppers", "Onions", "Hung Curd", "Spices"] },
            tip: "Chia seeds and walnuts add essential Omega-3 fatty acids, reducing inflammation."
          },
          Friday: {
            focus: "High-Fiber Sustenance",
            breakfast: { name: "Vegetable Dalia", ingredients: ["Dalia (Broken Wheat)", "Carrots", "Peas", "Ghee"] },
            lunch: { name: "Leftover Tandoori Chicken Wrap", ingredients: ["Chicken Breast", "Bell Peppers", "Atta (Flour)", "Mint Chutney"] },
            dinner: { name: "Palak Paneer & Multigrain Rotis", ingredients: ["Spinach", "Paneer", "Onions", "Garlic", "Multigrain Atta"] },
            tip: "Palak Paneer is an exceptional source of iron, calcium, and lean vegetarian protein."
          },
          Saturday: {
            focus: "Mindful Weekend Comfort",
            breakfast: { name: "Paneer Bhurji & Parantha", ingredients: ["Paneer", "Onions", "Tomatoes", "Green Chilies", "Atta (Flour)"] },
            lunch: { name: "Leftover Palak Paneer & Jeera Rice", ingredients: ["Spinach", "Paneer", "Basmati Rice"] },
            dinner: { name: "Chicken Tikka Skewers & Sprout Salad", ingredients: ["Chicken Breast", "Sprouted Moong", "Lemon", "Onions", "Cucumber"] },
            tip: "Sprouting moong dal increases its vitamin C content and makes it significantly easier to digest."
          },
          Sunday: {
            focus: "Rest & Reset Digestion",
            breakfast: { name: "Besan Chilla", ingredients: ["Besan (Gram Flour)", "Onions", "Green Chilies", "Curd (Yogurt)"] },
            lunch: { name: "Leftover Chicken Tikka & Sprout Salad", ingredients: ["Chicken Breast", "Sprouted Moong", "Cucumber"] },
            dinner: { name: "Light Lauki Kofta & Moong Dal", ingredients: ["Lauki (Bottle Gourd)", "Moong Dal", "Tomatoes", "Onions", "Atta (Flour)"] },
            tip: "Lauki (bottle gourd) is incredibly cooling and light on the digestive tract for Sunday evening recovery."
          }
        };

        const ingredientCategories = {
          "Chicken Breast": "Proteins & Poultry", "Eggs": "Proteins & Poultry",
          "Low-fat Paneer": "Dairy & Alternatives", "Paneer": "Dairy & Alternatives",
          "Curd (Yogurt)": "Dairy & Alternatives", "Greek Yogurt": "Dairy & Alternatives", "Hung Curd": "Dairy & Alternatives",
          "Spinach": "Fresh Produce", "Tomatoes": "Fresh Produce", "Onions": "Fresh Produce", "Cucumber": "Fresh Produce",
          "Broccoli": "Fresh Produce", "Green Beans": "Fresh Produce", "Carrots": "Fresh Produce", "Peas": "Fresh Produce",
          "Potatoes": "Fresh Produce", "Bell Peppers": "Fresh Produce", "Mixed Greens": "Fresh Produce", "Green Chilies": "Fresh Produce",
          "Garlic": "Fresh Produce", "Lauki (Bottle Gourd)": "Fresh Produce", "Pomegranate Seeds": "Fresh Produce", "Lemon": "Fresh Produce",
          "Yellow Lentils": "Pantry Staples & Grains", "Moong Dal": "Pantry Staples & Grains", "Black Chana (Chickpeas)": "Pantry Staples & Grains",
          "Sprouted Moong": "Pantry Staples & Grains", "Atta (Flour)": "Pantry Staples & Grains", "Multigrain Atta": "Pantry Staples & Grains",
          "Besan (Gram Flour)": "Pantry Staples & Grains", "Oats": "Pantry Staples & Grains", "Brown Rice": "Pantry Staples & Grains",
          "Basmati Rice": "Pantry Staples & Grains", "Dalia (Broken Wheat)": "Pantry Staples & Grains", "Multi-grain Bread": "Pantry Staples & Grains",
          "Peanuts": "Pantry Staples & Grains", "Walnuts": "Pantry Staples & Grains", "Chia Seeds": "Pantry Staples & Grains",
          "Ginger-Garlic Paste": "Pantry Staples & Grains", "Mint Chutney": "Pantry Staples & Grains", "Ghee": "Pantry Staples & Grains", "Spices": "Pantry Staples & Grains"
        };

        function App() {
          const [activeTab, setActiveTab] = useState('planner');
          const [activeDay, setActiveDay] = useState('Monday');

          const [completedMeals, setCompletedMeals] = useState(() => {
            const saved = localStorage.getItem('humsafar_completed_meals');
            return saved ? JSON.parse(saved) : {};
          });

          const [cartItems, setCartItems] = useState(() => {
            const saved = localStorage.getItem('humsafar_cart_items');
            return saved ? JSON.parse(saved) : {};
          });

          useEffect(() => {
            localStorage.setItem('humsafar_completed_meals', JSON.stringify(completedMeals));
          }, [completedMeals]);

          useEffect(() => {
            localStorage.setItem('humsafar_cart_items', JSON.stringify(cartItems));
          }, [cartItems]);

          const toggleMeal = (day, mealType) => {
            const key = `${day}-${mealType}`;
            setCompletedMeals(prev => ({ ...prev, [key]: !prev[key] }));
          };

          const toggleCartItem = (item) => {
            setCartItems(prev => ({ ...prev, [item]: !prev[item] }));
          };

          const resetEntireWeek = () => {
            if (window.confirm("Are you sure you want to clear your data and start a new week?")) {
              setCompletedMeals({});
              setCartItems({});
            }
          };

          const generateGroceryList = () => {
            const masterList = {};
            Object.keys(mealPlanData).forEach(day => {
              ['breakfast', 'lunch', 'dinner'].forEach(mealType => {
                const ingredients = mealPlanData[day][mealType].ingredients;
                ingredients.forEach(item => {
                  const category = ingredientCategories[item] || "Other Staples";
                  if (!masterList[category]) masterList[category] = new Set();
                  masterList[category].add(item);
                });
              });
            });
            const formattedList = {};
            Object.keys(masterList).forEach(cat => {
              formattedList[cat] = Array.from(masterList[cat]).sort();
            });
            return formattedList;
          };

          const groceryList = generateGroceryList();
          const currentDayData = mealPlanData[activeDay];

          return (
            <div className="max-w-md mx-auto my-6 bg-gray-50 min-h-screen shadow-2xl rounded-3xl overflow-hidden border border-gray-100 flex flex-col font-sans">
              <div className="bg-white p-6 pb-4 border-b border-gray-100">
                <div className="flex justify-between items-center mb-4">
                  <div>
                    <h1 className="text-2xl font-bold text-gray-900 tracking-tight">Humsafar Meals</h1>
                    <p className="text-xs font-semibold text-gray-500 tracking-wide uppercase">Late 30s Healthy Track</p>
                  </div>
                  <button 
                    onClick={resetEntireWeek}
                    className="text-[10px] uppercase font-bold tracking-wider text-red-500 hover:text-red-700 bg-red-50 hover:bg-red-100 px-2.5 py-1.5 rounded-lg transition-colors"
                  >
                    🔄 Reset Week
                  </button>
                </div>

                <div className="flex bg-gray-100 p-1 rounded-xl">
                  <button
                    onClick={() => setActiveTab('planner')}
                    className={`flex-1 py-2 text-center text-sm font-bold rounded-lg transition-all ${
                      activeTab === 'planner' ? 'bg-white text-gray-900 shadow-sm' : 'text-gray-500 hover:text-gray-900'
                    }`}
                  >
                    🗓️ Weekly Plan
                  </button>
                  <button
                    onClick={() => setActiveTab('grocery')}
                    className={`flex-1 py-2 text-center text-sm font-bold rounded-lg transition-all ${
                      activeTab === 'grocery' ? 'bg-white text-gray-900 shadow-sm' : 'text-gray-500 hover:text-gray-900'
                    }`}
                  >
                    🛒 Grocery List
                  </button>
                </div>
              </div>

              <div className="flex-1 p-6 overflow-y-auto">
                {activeTab === 'planner' && (
                  <div>
                    <div className="flex space-x-2 overflow-x-auto pb-3 mb-4">
                      {Object.keys(mealPlanData).map((day) => (
                        <button
                          key={day}
                          onClick={() => setActiveDay(day)}
                          className={`px-4 py-2 text-xs font-bold rounded-xl whitespace-nowrap transition-all ${
                            activeDay === day ? 'bg-orange-500 text-white shadow-md' : 'bg-white text-gray-600 border border-gray-100'
                          }`}
                        >
                          {day}
                        </button>
                      ))}
                    </div>

                    <div className="bg-orange-50 border-l-4 border-orange-500 p-4 mb-4 rounded-xl">
                      <span className="text-[10px] font-black uppercase tracking-wider text-orange-600 block mb-0.5">Target Dynamic</span>
                      <p className="text-sm font-bold text-gray-800">{currentDayData.focus}</p>
                    </div>

                    <div className="space-y-3">
                      {['breakfast', 'lunch', 'dinner'].map((mealType) => {
                        const isDone = completedMeals[`${activeDay}-${mealType}`];
                        return (
                          <div
                            key={mealType}
                            onClick={() => toggleMeal(activeDay, mealType)}
                            className={`p-4 rounded-2xl border transition-all cursor-pointer flex items-start space-x-3 ${
                              isDone ? 'bg-gray-100/70 border-gray-200 opacity-60' : 'bg-white border-gray-100 shadow-sm'
                            }`}
                          >
                            <input
                              type="checkbox"
                              checked={!!isDone}
                              readOnly
                              className="mt-1 h-4 w-4 text-orange-500 rounded border-gray-300"
                            />
                            <div>
                              <h3 className="text-xs font-black uppercase tracking-wider text-gray-400">{mealType}</h3>
                              <h4 className="text-sm font-bold text-gray-900 mt-0.5">{currentDayData[mealType].name}</h4>
                              <p className="text-xs text-gray-500 mt-1">
                                {currentDayData[mealType].ingredients.join(', ')}
                              </p>
                            </div>
                          </div>
                        );
                      })}
                    </div>

                    <div className="mt-6 bg-white border border-gray-100 rounded-2xl p-4 text-xs text-gray-600 shadow-sm">
                      <span className="font-bold text-gray-900 block mb-1">💡 Longevity & Health Insight</span>
                      {currentDayData.tip}
                    </div>
                  </div>
                )}

                {activeTab === 'grocery' && (
                  <div className="space-y-6">
                    <div className="bg-blue-50 border-l-4 border-blue-500 p-4 rounded-xl">
                      <p className="text-xs font-bold text-blue-800">Smart Storage Active</p>
                      <p className="text-xs text-blue-700 mt-0.5">Items remain crossed off even if you exit the browser inside the supermarket.</p>
                    </div>

                    {Object.keys(groceryList).map((category) => (
                      <div key={category} className="bg-white rounded-2xl border border-gray-100 p-4 shadow-sm">
                        <h3 className="text-xs font-black uppercase tracking-wider text-gray-400 mb-3 border-b border-gray-50 pb-1">
                          {category}
                        </h3>
                        <div className="grid grid-cols-1 gap-2.5">
                          {groceryList[category].map((item) => {
                            const isBought = cartItems[item];
                            return (
                              <div
                                key={item}
                                onClick={() => toggleCartItem(item)}
                                className="flex items-center space-x-3 cursor-pointer py-1"
                              >
                                <input
                                  type="checkbox"
                                  checked={!!isBought}
                                  readOnly
                                  className="h-4 w-4 text-blue-600 rounded border-gray-300"
                                />
                                <span className={`text-sm font-medium ${isBought ? 'line-through text-gray-300' : 'text-gray-700'}`}>
                                  {item}
                                </span>
                              </div>
                            );
                          })}
                        </div>
                      </div>
                    ))}
                  </div>
                )}
              </div>
            </div>
          );
        }

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>


