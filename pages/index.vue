<template>
  <main class="mb-16">
    <nav class="flex justify-between items-center w-full px-20 mx-auto">
      <h1>Kategoriyalar</h1>
      <div class="flex items-center">
        <svg
          class="w-6 h-6 stroke-current cursor-pointer"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"
          />
        </svg>
        <select v-model="store.search" class="w-5 outline-none">
          <option value="name">name</option>
          <option value="category">category</option>
        </select>
        <input
          @input="searchInput"
          v-model="store.searchinput"
          class="border-2 py-1 px-3 w-80 m-1 rounded-lg focus:outline-0"
          placeholder="Qidirish"
          type="search"
        />
        <button
          @click="createModal = true"
          class="p-1 bg-blue-600 border border-blue-600 active:bg-white active:text-blue-600 rounded-lg text-white px-5"
        >
          Yaratish
        </button>
      </div>
    </nav>
    <table
      id="table"
      class="draggable-table mb-20 pb-20 w-[100%] rounded-lg overflow-hidden"
    >
      <thead>
        <th>Emoji</th>
        <th>Nom</th>
        <th>Asosiy kategoriya</th>
        <th>Mavjudligi</th>
        <th>Harakatlar</th>
      </thead>
      <tbody v-if="!store.searchinput">
        <tr v-for="i in tableStore" :key="i.id">
          <td>📘</td>
          <td>{{ i.name }}</td>
          <td>{{ i.category }}</td>
          <td>
            <label class="switch">
              <input v-model="i.is_active" type="checkbox" />
              <span class="slider round"></span>
            </label>
          </td>
          <td>
            <div class="flex items-center wy justify-center">
              <img
                class="h-5 w-5 cursor-pointer"
                src="./../assets/delete.svg"
                alt=""
              />
              <i
                @click="() => editFunc(i.id)"
                class="text-blue-600 text-lg cursor-pointer bx bx-edit"
              ></i>
              <i
                @click="
                  deleteModal = true;
                  store.deleteId = i.id;
                "
                class="text-blue-600 text-lg cursor-pointer bx bx-trash"
              ></i>
            </div>
          </td>
        </tr>
      </tbody>
      <tbody v-else>
        <tr v-for="i in tableStore2" :key="i.id">
          <td>📘</td>
          <td>{{ i.name }}</td>
          <td>{{ i.category }}</td>
          <td>
            <label class="switch">
              <input v-model="i.is_active" type="checkbox" />
              <span class="slider round"></span>
            </label>
          </td>
          <td>
            <div class="flex items-center wy justify-center">
              <img
                class="h-5 w-5 cursor-pointer"
                src="./../assets/delete.svg"
                alt=""
              />
              <i
                @click="() => editFunc(i.id)"
                class="text-blue-600 text-lg cursor-pointer bx bx-edit"
              ></i>
              <i
                @click="
                  deleteModal = true;
                  store.deleteId = i.id;
                "
                class="text-blue-600 text-lg cursor-pointer bx bx-trash"
              ></i>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <section>
      <div
        v-show="editModal"
        id="authentication-modal"
        tabindex="-1"
        aria-hidden="true"
        class="flex fixed top-0 justify-center items-center min-h-screen bg-[#80808003] backdrop-blur-[1px] z-50 w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full"
      >
        <div class="relative w-full max-w-md max-h-full">
          <!-- Modal content -->
          <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
            <button
              @click="editModal = false"
              type="button"
              class="absolute top-3 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
              data-modal-hide="authentication-modal"
            >
              <i class="bx bx-x text-2xl font-bold"></i>
            </button>
            <div class="px-6 py-6 lg:px-8">
              <h3
                class="mb-4 text-xl font-medium text-gray-900 dark:text-white"
              >
                Tahrirlash
              </h3>
              <form @submit.prevent="handleEdit" class="space-y-6">
                <div>
                  <label
                    for="name"
                    class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
                    >Nom</label
                  >
                  <input
                    type="text"
                    name="name"
                    v-model="store.name"
                    id="name"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
                    placeholder="Apple"
                    required
                  />
                </div>
                <div>
                  <label
                    for="password"
                    class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
                    >Asosiy kategoriya</label
                  >
                  <select
                    v-model="store.category"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
                  >
                    <option value="Iphone">Iphone</option>
                    <option value="Samsung">Samsung</option>
                    <option value="RedMi">RedMi</option>
                  </select>
                </div>
                <button
                  type="submit"
                  class="w-full text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                >
                  Tahrirlash
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section>
      <div
        v-show="createModal"
        id="authentication-modal"
        tabindex="-1"
        aria-hidden="true"
        class="flex fixed top-0 justify-center items-center min-h-screen bg-[#80808003] backdrop-blur-[1px] z-50 w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full"
      >
        <div class="relative w-full max-w-md max-h-full">
          <!-- Modal content -->
          <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
            <button
              @click="createModal = false"
              type="button"
              class="absolute top-3 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
              data-modal-hide="authentication-modal"
            >
              <i class="bx bx-x text-2xl font-bold"></i>
            </button>
            <div class="px-6 py-6 lg:px-8">
              <h3
                class="mb-4 text-xl font-medium text-gray-900 dark:text-white"
              >
                Mahsulot yaratish
              </h3>
              <form @submit.prevent="handleSubmit" class="space-y-6" action="#">
                <div>
                  <label
                    for="name"
                    class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
                    >Nom</label
                  >
                  <input
                    v-model="form.name"
                    type="text"
                    name="name"
                    id="name"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
                    placeholder="Apple"
                    required
                  />
                </div>
                <div>
                  <label
                    for="password"
                    class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
                    >Asosiy kategoriya</label
                  >
                  <select
                    v-model="form.category"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
                    required
                  >
                    <option value="Iphone">Iphone</option>
                    <option value="Samsung">Samsung</option>
                    <option value="RedMi">RedMi</option>
                  </select>
                </div>
                <button
                  type="submit"
                  class="w-full text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                >
                  Yaratish
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section>
      <div
        v-show="deleteModal"
        id="popup-modal"
        tabindex="-1"
        class="flex fixed top-0 justify-center items-center min-h-screen bg-[#80808003] backdrop-blur-[1px] z-50 w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full"
      >
        <div class="relative w-full max-w-md max-h-full">
          <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
            <button
              type="button"
              class="absolute top-3 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
              data-modal-hide="popup-modal"
            >
              <i class="bx bx-xl"></i>
            </button>
            <div class="p-6 text-center">
              <svg
                class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 20 20"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M10 11V6m0 8h.01M19 10a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
                />
              </svg>
              <h3
                class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400"
              >
                Siz bu mahsulotni o'chirishni xohlaysizmi?
              </h3>
              <button
                @click="() => deleteFunc()"
                data-modal-hide="popup-modal"
                type="button"
                class="text-white bg-red-600 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 dark:focus:ring-red-800 font-medium rounded-lg text-sm inline-flex items-center px-5 py-2.5 text-center mr-2"
              >
                Ha
              </button>
              <button
                @click="deleteModal = false"
                data-modal-hide="popup-modal"
                type="button"
                class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-gray-200 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600"
              >
                Yo'q
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
const editModal = ref(false);
const createModal = ref(false);
const deleteModal = ref(false);
const tableStore2 = ref([]);

const store = reactive({
  editId: "",
  deleteId: "",
  name: "",
  category: "",
  search: "name",
  searchinput: "",
});

const form = reactive({
  name: "",
  category: "Iphone",
});

function editFunc(id) {
  editModal.value = true;
  store.editId = id;
  for (let i of tableStore.value) {
    console.log(i);
    if (i.id == id) {
      store.name = i.name;
      store.category = i.category;
      return;
    }
  }
}

function handleEdit() {
  for (let i of tableStore.value) {
    if (i.id == store.editId) {
      i.name = store.name;
      i.category = store.category;
    }
  }
  editModal.value = false;
}

function handleSubmit() {
  const data = {
    name: form.name,
    category: form.category,
    is_active: false,
  };
  tableStore.value.push(data);
  createModal.value = false;
  form.name = ""
  form.category = "Iphone"
}

function deleteFunc() {
  deleteModal.value = false;
  for (let i in tableStore.value) {
    console.log(i);
    if (tableStore.value[i].id == store.deleteId) {
      tableStore.value.splice(i, 1);
      console.log(tableStore.value);
      return;
    }
  }
  return true;
}

function searchInput() {
  tableStore2.value = [];
  for (let i of tableStore.value) {
    console.log(i[store.search].toLowerCase().indexOf(store.searchinput));
    if (i[store.search].toLowerCase().indexOf(store.searchinput) !== -1) {
      tableStore2.value.push(i);
    }
  }

  console.log(tableStore2.value);
}

const tableStore = ref([
  {
    id: "01H767NYK9XMTV3H817SYZR8VV",
    name: "tinyurl.com",
    category: "RedMi",
    is_active: true,
  },
  {
    id: "01H767NYKA0AXZMB9JN4PE09XQ",
    name: "amazon.co.jp",
    category: "Samsung",
    is_active: false,
  },
  {
    id: "01H767NYKBCNJ353Y5WFZFJ4RP",
    name: "dion.ne.jp",
    category: "Iphone",
    is_active: true,
  },
  {
    id: "01H767NYKCDK795AA69WVERKYY",
    name: "friendfeed.com",
    category: "RedMi",
    is_active: true,
  },
  {
    id: "01H767NYKC73T9ACGY4CW8Z9JX",
    name: "washingtonpost.com",
    category: "Samsung",
    is_active: false,
  },
  {
    id: "01H767NYKDM3883R05B8C8FQNN",
    name: "netvibes.com",
    category: "Iphone",
    is_active: false,
  },
  {
    id: "01H767NYKE0F5CC8ZYVQM4AEZG",
    name: "epa.gov",
    category: "Samsung",
    is_active: false,
  },
  {
    id: "01H767NYKF0GA41DN57CGQHCES",
    name: "symantec.com",
    category: "RedMi",
    is_active: true,
  },
  {
    id: "01H767NYKGH9FFEEFKH8WKJY5M",
    name: "istockphoto.com",
    category: "Iphone",
    is_active: true,
  },
  {
    id: "01H767NYKH2BMXWE3FBZZSQN6F",
    name: "chronoengine.com",
    category: "RedMi",
    is_active: true,
  },
]);

onMounted(() => {
  (function () {
    const table = document.getElementById("table");
    const tbody = table.querySelector("tbody");

    var currRow = null,
      dragElem = null,
      mouseDownX = 0,
      mouseDownY = 0,
      mouseX = 0,
      mouseY = 0,
      mouseDrag = false;

    function init() {
      bindMouse();
    }

    function bindMouse() {
      document.addEventListener("mousedown", (event) => {
        if (event.button != 0) return true;

        let target = getTargetRow(event.target);
        if (target) {
          currRow = target;
          addDraggableRow(target);
          currRow.classList.add("is-dragging");

          let coords = getMouseCoords(event);
          mouseDownX = coords.x;
          mouseDownY = coords.y;

          mouseDrag = true;
        }
      });

      document.addEventListener("mousemove", (event) => {
        if (!mouseDrag) return;

        let coords = getMouseCoords(event);
        // mouseX = coords.x - mouseDownX;
        mouseY = coords.y - mouseDownY;

        moveRow(mouseX, mouseY);
      });

      document.addEventListener("mouseup", (event) => {
        if (!mouseDrag) return;

        currRow.classList.remove("is-dragging");
        table.removeChild(dragElem);

        dragElem = null;
        mouseDrag = false;
      });
    }

    function swapRow(row, index) {
      let currIndex = Array.from(tbody.children).indexOf(currRow),
        row1 = currIndex > index ? currRow : row,
        row2 = currIndex > index ? row : currRow;

      tbody.insertBefore(row1, row2);
    }

    function moveRow(x, y) {
      dragElem.style.transform = "translate3d(" + x + "px, " + y + "px, 0)";

      let dPos = dragElem.getBoundingClientRect(),
        currStartY = dPos.y,
        currEndY = currStartY + dPos.height,
        rows = getRows();

      for (var i = 0; i < rows.length; i++) {
        let rowElem = rows[i],
          rowSize = rowElem.getBoundingClientRect(),
          rowStartY = rowSize.y,
          rowEndY = rowStartY + rowSize.height;

        if (
          currRow !== rowElem &&
          isIntersecting(currStartY, currEndY, rowStartY, rowEndY)
        ) {
          if (Math.abs(currStartY - rowStartY) < rowSize.height / 2)
            swapRow(rowElem, i);
        }
      }
    }

    function addDraggableRow(target) {
      dragElem = target.cloneNode(true);
      dragElem.classList.add("draggable-table__drag");
      dragElem.style.height = getStyle(target, "height");
      dragElem.style.background = getStyle(target, "backgroundColor");
      for (var i = 0; i < target.children.length; i++) {
        let oldTD = target.children[i],
          newTD = dragElem.children[i];
        newTD.style.width = getStyle(oldTD, "width");
        newTD.style.height = getStyle(oldTD, "height");
        newTD.style.padding = getStyle(oldTD, "padding");
        newTD.style.margin = getStyle(oldTD, "margin");
      }

      table.appendChild(dragElem);

      let tPos = target.getBoundingClientRect(),
        dPos = dragElem.getBoundingClientRect();
      dragElem.style.bottom = dPos.y - tPos.y - tPos.height + "px";
      dragElem.style.left = "-1px";

      document.dispatchEvent(
        new MouseEvent("mousemove", {
          view: window,
          cancelable: true,
          bubbles: true,
        })
      );
    }

    function getRows() {
      return table.querySelectorAll("tbody tr");
    }

    function getTargetRow(target) {
      let elemName = target.tagName.toLowerCase();

      if (elemName == "tr") return target;
      if (elemName == "td") return target.closest("tr");
    }

    function getMouseCoords(event) {
      return {
        x: event.clientX,
        y: event.clientY,
      };
    }

    function getStyle(target, styleName) {
      let compStyle = getComputedStyle(target),
        style = compStyle[styleName];

      return style ? style : null;
    }

    function isIntersecting(min0, max0, min1, max1) {
      return (
        Math.max(min0, max0) >= Math.min(min1, max1) &&
        Math.min(min0, max0) <= Math.max(min1, max1)
      );
    }

    init();
  })();
});
</script>

<style lang="scss" scoped>
* {
  font-family: "Source Sans Pro", sans-serif;
}

p {
  font-size: 0.75em;
  font-weight: bold;
  position: absolute;
  width: 100%;
  letter-spacing: 5px;
  text-transform: uppercase;
  text-align: center;
  color: white;
  user-select: none;
}

nav {
  width: 90%;
  top: 0;
  z-index: 10;
  margin: 0 auto 0;
  position: sticky;
  padding: 10px 30px;
  color: black;
  border-collapse: collapse;
  background: white;
  -webkit-box-shadow: 0px 0px 10px 8px rgba(0, 0, 0, 0.1);
  box-shadow: 0px 0px 4px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}
.draggable-table {
  width: 90%;
  position: relative;
  top: 2rem;
  margin: 0 auto;
  height: 50%;
  border-collapse: collapse;
  background: white;
  -webkit-box-shadow: 0px 0px 10px 8px rgba(0, 0, 0, 0.1);
  box-shadow: 0px 0px 4px 4px rgba(0, 0, 0, 0.1);
}

img {
  height: 50px;
  border-radius: 10px;
}

.draggable-table .draggable-table__drag {
  font-size: 0.95em;
  font-weight: lighter;
  text-transform: capitalize;
  position: absolute;
  width: 100%;
  text-indent: 20px;
  border: 1px solid #f1f1f1;
  z-index: 10;
  cursor: grabbing;
  -webkit-box-shadow: 8px 8px 8px 8px rgba(0, 0, 0, 0.615);
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.354);
  opacity: 1;
}

.draggable-table thead th {
  height: 25px;
  font-weight: bold;
  text-transform: capitalize;
  padding: 10px;
  user-select: none;
  text-align: start;
  padding-left: 2rem;
}

.draggable-table tbody tr {
  cursor: pointer;
}

.draggable-table tbody tr td {
  font-size: 0.95em;
  font-weight: lighter;
  text-transform: capitalize;
  text-indent: 20px;
  padding: 10px;
  user-select: none;
  border-top: 1px solid whitesmoke;
}

.draggable-table tbody tr:nth-child(even) {
  background-color: #f7f7f7;
}

.draggable-table tbody tr:nth-child(odd) {
  background-color: #ffffff;
}

.draggable-table tbody tr.is-dragging {
  background: white;
  box-shadow: 8px 8px 8px solid grey;
}

.draggable-table tbody tr.is-dragging td {
  color: white;
}

/* ------------------------------------------------- */
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  transform: scale(0.6);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #2196f3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196f3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>
