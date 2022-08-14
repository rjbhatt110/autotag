<template>
  <div class="bg-gray-100">
    <div class="w-full">
      <div>
        <TransitionRoot as="template" :show="sidebarOpen">
          <Dialog
            as="div"
            class="relative z-40 md:hidden"
            @close="sidebarOpen = false"
          >
            <TransitionChild
              as="template"
              enter="transition-opacity ease-linear duration-300"
              enter-from="opacity-0"
              enter-to="opacity-100"
              leave="transition-opacity ease-linear duration-300"
              leave-from="opacity-100"
              leave-to="opacity-0"
            >
              <div class="fixed inset-0 bg-gray-600 bg-opacity-75" />
            </TransitionChild>

            <div class="fixed inset-0 flex z-40">
              <TransitionChild
                as="template"
                enter="transition ease-in-out duration-300 transform"
                enter-from="-translate-x-full"
                enter-to="translate-x-0"
                leave="transition ease-in-out duration-300 transform"
                leave-from="translate-x-0"
                leave-to="-translate-x-full"
              >
                <DialogPanel
                  class="relative flex-1 flex flex-col max-w-xs w-full bg-white"
                >
                  <TransitionChild
                    as="template"
                    enter="ease-in-out duration-300"
                    enter-from="opacity-0"
                    enter-to="opacity-100"
                    leave="ease-in-out duration-300"
                    leave-from="opacity-100"
                    leave-to="opacity-0"
                  >
                    <div class="absolute top-0 right-0 -mr-12 pt-2">
                      <button
                        type="button"
                        class="ml-1 flex items-center justify-center h-10 w-10 rounded-full focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white"
                        @click="sidebarOpen = false"
                      >
                        <span class="sr-only">Close sidebar</span>
                        <XIcon class="h-6 w-6 text-white" aria-hidden="true" />
                      </button>
                    </div>
                  </TransitionChild>
                  <div class="flex-1 h-0 pt-5 pb-4 overflow-y-auto">
                    <nav class="mt-5 px-2 space-y-1">
                      <a
                        v-for="item in navigation"
                        :key="item.name"
                        :href="item.href"
                        :class="[
                          item.current
                            ? 'bg-gray-100 text-gray-900'
                            : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900',
                          'group flex items-center px-2 py-2 text-base font-medium rounded-md',
                        ]"
                      >
                        <component
                          :is="item.icon"
                          :class="[
                            item.current
                              ? 'text-gray-500'
                              : 'text-gray-400 group-hover:text-gray-500',
                            'mr-4 flex-shrink-0 h-6 w-6',
                          ]"
                          aria-hidden="true"
                        />
                        {{ item.name }}
                      </a>
                    </nav>
                  </div>
                </DialogPanel>
              </TransitionChild>
              <div class="flex-shrink-0 w-14">
                <!-- Force sidebar to shrink to fit close icon -->
              </div>
            </div>
          </Dialog>
        </TransitionRoot>

        <!-- Static sidebar for desktop -->
        <div
          class=""
          :class="[
            collapse ? 'md:w-64 duration-500' : 'md:w-14 duration-500',
            'hidden md:flex md:flex-col md:fixed md:inset-y-0',
          ]"
        >
          <!-- Sidebar component, swap this element with another sidebar if you like -->
          <div
            class="flex-1 flex flex-col min-h-0 pb-4 border-r border-gray-200 bg-white"
          >
            <!-- Full Sidebar -->
            <nav class="mt-5 flex-1 px-2 bg-white space-y-1">
              <a
                v-for="item in navigation"
                :key="item.name"
                :href="item.href"
                :class="[
                  item.current
                    ? 'bg-gray-100 text-gray-900'
                    : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900',
                  'group flex items-center px-2 py-2 text-sm font-medium rounded-md',
                ]"
              >
                <component
                  :is="item.icon"
                  :class="[
                    item.current
                      ? 'text-gray-500'
                      : 'text-gray-400 group-hover:text-gray-500',
                    'flex-shrink-0 h-6 w-6',
                  ]"
                  aria-hidden="true"
                />
                <span :class="[!collapse ? 'sr-only duration-500' : 'ml-3']">{{
                  item.name
                }}</span>
              </a>
            </nav>

            <div
              class="flex-shrink-0 flex border-t border-gray-200 pt-4 pr-4 pl-4"
            >
              <a href="#" class="flex-shrink-0 w-full group block">
                <div class="flex items-center">
                  <ArrowLeftIcon
                    v-if="collapse"
                    @click="menuCollapse()"
                    class="text-gray-400 flex-shrink-0 h-6 w-6"
                  />
                  <ArrowRightIcon
                    v-if="!collapse"
                    @click="menuCollapse()"
                    class="text-gray-400 flex-shrink-0 h-6 w-6"
                  />
                </div>
              </a>
            </div>
          </div>
        </div>
        <div :class="[collapse ? 'md:pl-64 duration-500' : 'md:pl-16 duration-500', 'flex flex-col flex-1']">
          <!-- Mobile top bar -->
          <div
            class="sticky top-0 z-10 md:hidden pl-1 pt-1 sm:pl-3 sm:pt-3 bg-white"
          >
            <button
              type="button"
              class="-ml-0.5 -mt-0.5 h-12 w-12 inline-flex items-center justify-center rounded-md text-gray-500 hover:text-gray-900 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-indigo-500"
              @click="sidebarOpen = true"
            >
              <span class="sr-only">Open sidebar</span>
              <MenuIcon class="h-6 w-6" aria-hidden="true" />
            </button>
          </div>

          <main class="flex-1">
            <div class="p-6">
              <div class="w-full mb-4 sm:px-6 md:px-8">
                <div class="md:flex md:items-center md:justify-between">
                  <div class="flex items-center flex-1 min-w-0">
                    <div class="relative">
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        x="0px"
                        y="0px"
                        width="48"
                        height="48"
                        viewBox="0 0 48 48"
                        style="fill: #000000"
                      >
                        <path
                          fill="#4caf50"
                          d="M37,39H11l-6,6V11c0-3.3,2.7-6,6-6h26c3.3,0,6,2.7,6,6v22C43,36.3,40.3,39,37,39z"
                        ></path>
                        <path
                          fill="#004d40"
                          d="M33.414,29.586l-7.636-7.636l-2.828,2.828l7.636,7.636C30.977,32.805,31.488,33,32,33 c0.512,0,1.023-0.195,1.414-0.586S34,31.512,34,31S33.805,29.977,33.414,29.586z"
                        ></path>
                        <path
                          fill="#e0e0e0"
                          d="M25.21 22.79L23.79 24.21 16.59 17 15.14 17 13 13.43 14.43 12 18 14.14 18 15.59z"
                        ></path>
                        <path
                          fill="#bdbdbd"
                          d="M25.2,22.79l-3.32-3.32l-1.41,1.41l3.32,3.33L25.2,22.79z"
                        ></path>
                        <path
                          fill="#e0e0e0"
                          d="M36,16.5c0,3.04-2.46,5.5-5.5,5.5c-0.79,0-1.53-0.17-2.21-0.46L17.41,32.41 C17.02,32.8,16.51,33,16,33s-1.02-0.2-1.41-0.59c-0.395-0.39-0.593-0.9-0.593-1.41c0-0.51,0.198-1.02,0.593-1.41l10.87-10.88 C25.17,18.03,25,17.29,25,16.5c0-3.04,2.46-5.5,5.5-5.5c0.79,0,1.53,0.17,2.21,0.46l-4.7,4.7l2.83,2.83l4.7-4.7 C35.83,14.97,36,15.71,36,16.5z"
                        ></path>
                      </svg>
                    </div>
                    <h1
                      class="text-2xl font-bold text-gray-900 sm:text-3xl sm:truncate"
                    >
                      Autotag node
                    </h1>
                  </div>
                  <div class="mt-4 flex md:mt-0 md:ml-4">
                    <button
                      type="button"
                      class="inline-flex items-center px-4 py-2 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                    >
                      Save
                    </button>
                    <button
                      type="button"
                      class="ml-3 inline-flex items-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                    >
                      Close
                    </button>

                    <div
                      class="ml-3 relative z-0 inline-flex shadow-sm rounded-md"
                    >
                      <Menu as="div" class="-ml-px relative block">
                        <MenuButton
                          class="relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50 focus:z-10 focus:outline-none focus:ring-1 focus:ring-indigo-500 focus:border-indigo-500"
                        >
                          <span class="sr-only">Open options</span>
                          <DotsVerticalIcon
                            class="h-5 w-5"
                            aria-hidden="true"
                          />
                        </MenuButton>
                        <transition
                          enter-active-class="transition ease-out duration-100"
                          enter-from-class="transform opacity-0 scale-95"
                          enter-to-class="transform opacity-100 scale-100"
                          leave-active-class="transition ease-in duration-75"
                          leave-from-class="transform opacity-100 scale-100"
                          leave-to-class="transform opacity-0 scale-95"
                        >
                          <MenuItems
                            class="origin-top-right absolute right-0 mt-2 -mr-1 w-56 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5 focus:outline-none"
                          >
                            <div class="py-1">
                              <MenuItem
                                v-for="item in items"
                                :key="item.name"
                                v-slot="{ active }"
                              >
                                <a
                                  :href="item.href"
                                  :class="[
                                    active
                                      ? 'bg-gray-100 text-gray-900'
                                      : 'text-gray-700',
                                    'block px-4 py-2 text-sm',
                                  ]"
                                >
                                  {{ item.name }}
                                </a>
                              </MenuItem>
                            </div>
                          </MenuItems>
                        </transition>
                      </Menu>
                    </div>
                  </div>
                </div>
              </div>
              <div class="w-full sm:px-6 md:px-8">
                <!-- Replace with your content -->
                <div class="space-y-6 lg:px-0 lg:col-span-9">
                  <form action="#" method="POST">
                    <div class="shadow sm:rounded-md sm:overflow-hidden">
                      <div class="bg-white px-4 space-y-6 sm:pb-6 sm:pr-6 sm:pl-6 sm:pt-3">
                        <!-- Tab -->
                        <div>
                          <div class="sm:block">
                            <div class="border-b border-gray-200">
                              <nav
                                class="-mb-px flex space-x-8"
                                aria-label="Tabs"
                              >
                                <a
                                  v-for="tab in tabs"
                                  :key="tab.name"
                                  :href="tab.href"
                                  :class="[
                                    tab.current
                                      ? 'border-indigo-500 text-indigo-600'
                                      : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300',
                                    'group inline-flex items-center py-4 px-1 border-b-2 font-medium text-sm',
                                  ]"
                                  :aria-current="
                                    tab.current ? 'page' : undefined
                                  "
                                >
                                  <component
                                    :is="tab.icon"
                                    :class="[
                                      tab.current
                                        ? 'text-indigo-500'
                                        : 'text-gray-400 group-hover:text-gray-500',
                                      '-ml-0.5 mr-2 h-5 w-5',
                                    ]"
                                    aria-hidden="true"
                                  />
                                  <span>{{ tab.name }}</span>
                                </a>
                              </nav>
                            </div>
                          </div>
                        </div>
                        <!-- Form Section -->
                        <div class="grid grid-cols-3 gap-6">
                          <!-- Subject -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <div class="flex flex-1 items-center">
                              <label
                                for="subject"
                                class="block text-sm font-medium text-gray-700 mr-1 mb-[2px]"
                                >Subject</label
                              >
                              <svg
                                xmlns="http://www.w3.org/2000/svg"
                                x="0px"
                                y="0px"
                                width="14"
                                height="14"
                                viewBox="0 0 172 172"
                                style="fill: #000000"
                              >
                                <g
                                  fill="none"
                                  fill-rule="nonzero"
                                  stroke="none"
                                  stroke-width="1"
                                  stroke-linecap="butt"
                                  stroke-linejoin="miter"
                                  stroke-miterlimit="10"
                                  stroke-dasharray=""
                                  stroke-dashoffset="0"
                                  font-family="none"
                                  font-weight="none"
                                  font-size="none"
                                  text-anchor="none"
                                  style="mix-blend-mode: normal"
                                >
                                  <path
                                    d="M0,172v-172h172v172z"
                                    fill="none"
                                  ></path>
                                  <g>
                                    <path
                                      d="M21.5,150.5v-129h129v129zM35.83333,136.16667h100.33333v-100.33333h-100.33333z"
                                      fill="#90caf9"
                                    ></path>
                                    <path
                                      d="M86,148.70833c-3.956,0 -7.16667,-3.21067 -7.16667,-7.16667v-73.45833h14.33333v73.45833c0,3.956 -3.21067,7.16667 -7.16667,7.16667z"
                                      fill="#90caf9"
                                    ></path>
                                    <path
                                      d="M139.75,75.25h-107.5c-3.956,0 -7.16667,-3.21067 -7.16667,-7.16667c0,-3.956 3.21067,-7.16667 7.16667,-7.16667h107.5c3.956,0 7.16667,3.21067 7.16667,7.16667c0,3.956 -3.21067,7.16667 -7.16667,7.16667zM139.75,111.08333h-107.5c-3.956,0 -7.16667,-3.21067 -7.16667,-7.16667c0,-3.956 3.21067,-7.16667 7.16667,-7.16667h107.5c3.956,0 7.16667,3.21067 7.16667,7.16667c0,3.956 -3.21067,7.16667 -7.16667,7.16667z"
                                      fill="#90caf9"
                                    ></path>
                                    <g fill="#2ecc71">
                                      <path
                                        d="M136.16667,35.83333v25.08333h-100.33333v-25.08333h100.33333M150.5,21.5h-129v53.75h129v-53.75z"
                                      ></path>
                                    </g>
                                    <path
                                      d="M78.83333,28.66667h14.33333v43h-14.33333z"
                                      fill="#2ecc71"
                                    ></path>
                                  </g>
                                </g>
                              </svg>
                            </div>
                            <input
                              type="text"
                              name="subject"
                              id="subject"
                              autocomplete="given-name"
                              value="Autotag node"
                              class="mt-1 md:ml-1.5 block flex-[4] w-full border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                            />
                          </div>
                          <!-- Company -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="company"
                              class="block text-sm flex-1 font-medium text-gray-700"
                              >Company</label
                            >
                            <div class="flex items-center flex-[4]">
                              <div
                                class="mt-1 relative rounded-md shadow-sm flex-1"
                              >
                                <div
                                  class="absolute inset-y-0 left-0 pl-3 pt-1 flex items-center pointer-events-none"
                                >
                                  <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    x="0px"
                                    y="0px"
                                    width="24"
                                    height="24"
                                    viewBox="0 0 48 48"
                                    style="fill: #000000"
                                  >
                                    <path
                                      fill="#C5CAE9"
                                      d="M42 42L6 42 6 9 24 2 42 9z"
                                    ></path>
                                    <path
                                      fill="#9FA8DA"
                                      d="M6 42H42V44H6z"
                                    ></path>
                                    <path
                                      fill="#BF360C"
                                      d="M20 35H28V44H20z"
                                    ></path>
                                    <path
                                      fill="#1565C0"
                                      d="M31 27H37V32H31zM21 27H27V32H21zM11 27H17V32H11zM31 35H37V40H31zM11 35H17V40H11zM31 19H37V24H31zM21 19H27V24H21zM11 19H17V24H11zM31 11H37V16H31zM21 11H27V16H21zM11 11H17V16H11z"
                                    ></path>
                                  </svg>
                                </div>
                                <select
                                  id="company"
                                  name="company"
                                  autocomplete="company-name"
                                  class="mt-1 block w-full bg-white border border-gray-300 rounded-md shadow-sm py-2 pl-10 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                                >
                                  <option>Supporters A/S</option>
                                  <option>Google</option>
                                  <option>Amazon</option>
                                </select>
                              </div>
                              <svg
                                xmlns="http://www.w3.org/2000/svg"
                                x="0px"
                                y="0px"
                                width="24"
                                height="24"
                                viewBox="0 0 32 32"
                                style="fill: #000000"
                                class="mt-2 ml-3"
                              >
                                <path
                                  d="M 6 4 L 6 28 L 26 28 L 26 4 Z M 8 6 L 24 6 L 24 26 L 8 26 Z M 10 9 L 10 11 L 22 11 L 22 9 Z M 10 13 L 10 15 L 12 15 L 12 13 Z M 14 13 L 14 15 L 22 15 L 22 13 Z M 10 17 L 10 19 L 12 19 L 12 17 Z M 14 17 L 14 19 L 22 19 L 22 17 Z M 10 21 L 10 23 L 12 23 L 12 21 Z M 14 21 L 14 23 L 22 23 L 22 21 Z"
                                ></path>
                              </svg>
                            </div>
                          </div>
                          <!-- Requested By -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <div class="flex flex-1 flex-wrap items-center">
                              <label
                                for="requested"
                                class="block text-sm font-medium mr-1 mb-[2px] text-gray-700"
                                >Requested By</label
                              >
                              <svg
                                xmlns="http://www.w3.org/2000/svg"
                                x="0px"
                                y="0px"
                                width="14"
                                height="14"
                                viewBox="0 0 48 48"
                                style="fill: #000000"
                              >
                                <path
                                  fill="#1976D2"
                                  d="M38 14h-3c-5.514 0-10 4.486-10 10s4.486 10 10 10h3c5.514 0 10-4.486 10-10S43.514 14 38 14M38 30h-3c-3.309 0-6-2.691-6-6s2.691-6 6-6h3c3.309 0 6 2.691 6 6S41.309 30 38 30M13 14h-3C4.486 14 0 18.486 0 24s4.486 10 10 10h3c5.514 0 10-4.486 10-10S18.514 14 13 14M13 30h-3c-3.309 0-6-2.691-6-6s2.691-6 6-6h3c3.309 0 6 2.691 6 6S16.309 30 13 30"
                                ></path>
                                <path
                                  fill="#42A5F5"
                                  d="M33,22H15c-1.104,0-2,0.896-2,2s0.896,2,2,2h18c1.104,0,2-0.896,2-2S34.104,22,33,22"
                                ></path>
                              </svg>
                            </div>
                            <div class="flex flex-[4] items-center">
                              <div
                                class="mt-1 relative rounded-md shadow-sm flex-1"
                              >
                                <div
                                  class="absolute inset-y-0 left-0 pl-3 pt-1 flex items-center pointer-events-none"
                                >
                                  <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    x="0px"
                                    y="0px"
                                    width="24"
                                    height="24"
                                    viewBox="0 0 48 48"
                                    style="fill: #000000"
                                  >
                                    <path
                                      fill="#4FC3F7"
                                      d="M29,31L29,31c0,0-1,4-5,4s-5-4-5-4S8,32.986,8,44h32C40,33.025,29,31,29,31"
                                    ></path>
                                    <path
                                      fill="#FF9800"
                                      d="M24,37c-5,0-5-6-5-6v-6h10v6C29,31,29,37,24,37z"
                                    ></path>
                                    <path
                                      fill="#FFA726"
                                      d="M35 19c0 1.105-.895 2-2 2s-2-.895-2-2 .895-2 2-2S35 17.895 35 19M17 19c0 1.105-.895 2-2 2s-2-.895-2-2 .895-2 2-2S17 17.895 17 19"
                                    ></path>
                                    <path
                                      fill="#FFB74D"
                                      d="M33,13c0-7.635-18-4.971-18,0v7c0,4.971,4.029,9,9,9c4.971,0,9-4.029,9-9V13z"
                                    ></path>
                                    <path
                                      fill="#424242"
                                      d="M24,4c-6.075,0-10,4.925-10,11v2.284L16,19v-5l12-4l4,4v5l2-1.741V15c0-4.025-1.038-8.015-6-9l-1-2H24z"
                                    ></path>
                                    <path
                                      fill="#784719"
                                      d="M27 19c0 .552.448 1 1 1s1-.448 1-1-.448-1-1-1S27 18.448 27 19M19 19c0 .552.448 1 1 1s1-.448 1-1-.448-1-1-1S19 18.448 19 19"
                                    ></path>
                                    <path
                                      fill="#01579B"
                                      d="M24,37c5,0,6.745-3.93,6.951-5.428C29.794,31.148,29,31,29,31s-1,4-5,4s-5-4-5-4s-0.794,0.146-1.952,0.566C17.252,33.061,19,37,24,37z"
                                    ></path>
                                  </svg>
                                </div>
                                <select
                                  id="requested"
                                  name="requested"
                                  autocomplete="requested-name"
                                  class="mt-1 block w-full bg-white border border-gray-300 rounded-md shadow-sm py-2 pl-10 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                                >
                                  <option>Jesper Jonsson</option>
                                  <option>Raj Bhatt</option>
                                  <option>Mike Ross</option>
                                </select>
                              </div>
                              <svg
                                xmlns="http://www.w3.org/2000/svg"
                                x="0px"
                                y="0px"
                                width="24"
                                height="24"
                                viewBox="0 0 32 32"
                                style="fill: #000000"
                                class="mt-2 ml-3"
                              >
                                <path
                                  d="M 6 4 L 6 28 L 26 28 L 26 4 Z M 8 6 L 24 6 L 24 26 L 8 26 Z M 10 9 L 10 11 L 22 11 L 22 9 Z M 10 13 L 10 15 L 12 15 L 12 13 Z M 14 13 L 14 15 L 22 15 L 22 13 Z M 10 17 L 10 19 L 12 19 L 12 17 Z M 14 17 L 14 19 L 22 19 L 22 17 Z M 10 21 L 10 23 L 12 23 L 12 21 Z M 14 21 L 14 23 L 22 23 L 22 21 Z"
                                ></path>
                              </svg>
                            </div>
                          </div>
                          <!-- Affected Divider -->
                          <div class="relative col-span-6 sm:col-span-3">
                            <div
                              class="absolute inset-0 md:left-[20%] flex items-center"
                              aria-hidden="true"
                            >
                              <div class="w-full border-t border-gray-300" />
                            </div>
                            <div
                              class="relative flex items-center justify-between"
                            >
                              <span
                                class="pr-3 bg-white text-sm font-medium text-gray-900"
                              >
                                Affects
                              </span>
                              <button
                                type="button"
                                class="inline-flex items-center shadow-sm px-2 py-1 border border-gray-300 text-sm leading-5 font-medium rounded-full text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                              >
                                <PlusSmIcon
                                  class="h-4 w-4 text-gray-400"
                                  aria-hidden="true"
                                />
                              </button>
                            </div>
                          </div>
                          <!-- Assign To -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="assigned"
                              class="block text-sm flex-1 font-medium text-gray-700"
                              >Assigned To</label
                            >
                            <div
                              class="mt-1 flex-[4] relative rounded-md shadow-sm"
                            >
                              <div
                                class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
                              >
                                <svg
                                  xmlns="http://www.w3.org/2000/svg"
                                  x="0px"
                                  y="0px"
                                  width="24"
                                  height="24"
                                  viewBox="0 0 48 48"
                                  style="fill: #000000"
                                >
                                  <path
                                    fill="#EF6C00"
                                    d="M38.028,8.729L35.2,5.9c0,0-2.121-2.121-4.242,0l-3.89,3.889c0.586,0.585,0.586,1.536,0,2.122c-0.586,0.585-1.535,0.585-2.121,0L8.999,27.916c-2.121,2.121,0,4.243,0,4.243l2.829,2.828c1.172-1.172,3.071-1.171,4.242,0c1.172,1.171,1.172,3.071,0,4.243l2.828,2.828c2.122,2.121,4.243,0,4.243,0s9.807-9.865,15.948-16.006c-0.586-0.586-0.586-1.536,0-2.121c0.585-0.585,1.535-0.585,2.121,0c1.813-1.813,3.242-3.242,3.889-3.889c2.121-2.121,0-4.243,0-4.243l-2.828-2.828c-1.172,1.172-3.071,1.171-4.243,0C36.857,11.8,36.856,9.901,38.028,8.729z M37.675,22.518c-0.585,0.585-1.535,0.585-2.121,0c-0.585-0.585-0.585-1.536,0-2.122c0.586-0.585,1.536-0.585,2.121,0C38.261,20.982,38.261,21.932,37.675,22.518z M34.14,18.982c-0.586,0.585-1.536,0.585-2.121,0c-0.586-0.585-0.586-1.536,0-2.121c0.585-0.586,1.535-0.586,2.121,0C34.725,17.446,34.725,18.396,34.14,18.982z M30.604,15.446c-0.585,0.586-1.535,0.586-2.121,0c-0.585-0.585-0.585-1.536,0-2.121c0.586-0.585,1.536-0.585,2.121,0C31.189,13.911,31.189,14.861,30.604,15.446z"
                                  ></path>
                                  <path
                                    fill="#FFA000"
                                    d="M31.986,9.729L29.157,6.9c0,0-2.121-2.121-4.242,0l-3.889,3.889c0.585,0.585,0.585,1.536,0,2.122c-0.586,0.585-1.536,0.585-2.122,0L2.958,28.916c-2.122,2.121,0,4.242,0,4.242l2.828,2.828c1.172-1.172,3.071-1.171,4.242,0c1.172,1.172,1.172,3.071,0,4.243l2.829,2.829c2.121,2.121,4.242,0,4.242,0s9.806-9.865,15.947-16.006c-0.586-0.585-0.586-1.535,0-2.121c0.585-0.585,1.535-0.585,2.121,0c1.813-1.813,3.242-3.242,3.889-3.889c2.122-2.121,0-4.242,0-4.242l-2.828-2.829c-1.172,1.172-3.071,1.171-4.242,0C30.814,12.8,30.814,10.901,31.986,9.729z M31.633,23.518c-0.586,0.585-1.536,0.585-2.122,0c-0.585-0.585-0.585-1.536,0-2.121c0.586-0.586,1.536-0.586,2.122,0C32.218,21.982,32.218,22.932,31.633,23.518z M28.097,19.982c-0.585,0.585-1.535,0.585-2.121,0s-0.586-1.536,0-2.121s1.536-0.585,2.121,0C28.683,18.446,28.683,19.396,28.097,19.982z M24.562,16.446c-0.586,0.586-1.536,0.586-2.121,0c-0.586-0.585-0.586-1.536,0-2.121c0.585-0.585,1.535-0.585,2.121,0S25.147,15.861,24.562,16.446z"
                                  ></path>
                                </svg>
                              </div>
                              <select
                                id="assigned"
                                name="assigned"
                                autocomplete="assigned-name"
                                class="mt-1 block w-full bg-white border border-gray-300 rounded-md shadow-sm py-2 pl-10 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                              >
                                <option>Denis S</option>
                                <option>Harvey S</option>
                                <option>Danial R</option>
                              </select>
                            </div>
                          </div>
                          <!-- Type -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="type"
                              class="block flex-1 text-sm font-medium text-gray-700"
                              >Type</label
                            >
                            <div
                              class="mt-1 flex-[4] relative rounded-md shadow-sm"
                            >
                              <div
                                class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
                              >
                                <svg
                                  xmlns="http://www.w3.org/2000/svg"
                                  x="0px"
                                  y="0px"
                                  width="24"
                                  height="24"
                                  viewBox="0 0 48 48"
                                  style="fill: #000000"
                                >
                                  <path
                                    fill="#4caf50"
                                    d="M37,39H11l-6,6V11c0-3.3,2.7-6,6-6h26c3.3,0,6,2.7,6,6v22C43,36.3,40.3,39,37,39z"
                                  ></path>
                                  <path
                                    fill="#004d40"
                                    d="M33.414,29.586l-7.636-7.636l-2.828,2.828l7.636,7.636C30.977,32.805,31.488,33,32,33 c0.512,0,1.023-0.195,1.414-0.586S34,31.512,34,31S33.805,29.977,33.414,29.586z"
                                  ></path>
                                  <path
                                    fill="#e0e0e0"
                                    d="M25.21 22.79L23.79 24.21 16.59 17 15.14 17 13 13.43 14.43 12 18 14.14 18 15.59z"
                                  ></path>
                                  <path
                                    fill="#bdbdbd"
                                    d="M25.2,22.79l-3.32-3.32l-1.41,1.41l3.32,3.33L25.2,22.79z"
                                  ></path>
                                  <path
                                    fill="#e0e0e0"
                                    d="M36,16.5c0,3.04-2.46,5.5-5.5,5.5c-0.79,0-1.53-0.17-2.21-0.46L17.41,32.41 C17.02,32.8,16.51,33,16,33s-1.02-0.2-1.41-0.59c-0.395-0.39-0.593-0.9-0.593-1.41c0-0.51,0.198-1.02,0.593-1.41l10.87-10.88 C25.17,18.03,25,17.29,25,16.5c0-3.04,2.46-5.5,5.5-5.5c0.79,0,1.53,0.17,2.21,0.46l-4.7,4.7l2.83,2.83l4.7-4.7 C35.83,14.97,36,15.71,36,16.5z"
                                  ></path>
                                </svg>
                              </div>
                              <select
                                id="type"
                                name="type"
                                autocomplete="type-name"
                                class="mt-1 block w-full bg-white border border-gray-300 rounded-md shadow-sm py-2 pl-10 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                              >
                                <option>Change Request</option>
                                <option>New Request</option>
                              </select>
                            </div>
                          </div>
                          <!-- Priority -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="priority"
                              class="block flex-1 text-sm font-medium text-gray-700"
                              >Priority</label
                            >
                            <select
                              id="priority"
                              name="priority"
                              autocomplete="priority-name"
                              class="mt-1 md:ml-1 flex-[4] block w-full bg-white border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                            >
                              <option>Low</option>
                              <option>Medium</option>
                              <option>High</option>
                            </select>
                          </div>
                          <!-- Status -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="status"
                              class="block flex-1 text-sm font-medium text-gray-700"
                              >Status</label
                            >
                            <select
                              id="status"
                              name="status"
                              autocomplete="status-name"
                              class="mt-1 md:ml-1 flex-[4] block w-full bg-white border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                            >
                              <option>Open</option>
                              <option>In Process</option>
                              <option>Close</option>
                            </select>
                          </div>
                          <!-- Estimated Time -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="estimate"
                              class="block flex-1 text-sm font-medium text-gray-700"
                              >Estimated Time</label
                            >
                            <input
                              type="number"
                              class="form-control flex-[4] md:ml-1 block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                              id="estimate"
                              value="0"
                            />
                          </div>
                          <!-- Channel -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="channel"
                              class="block flex-1 text-sm font-medium text-gray-700"
                              >Channel</label
                            >
                            <select
                              id="channel"
                              name="channel"
                              autocomplete="channel-name"
                              class="mt-1 md:ml-1 flex-[4] block w-full bg-white border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                            >
                              <option>Phone</option>
                              <option>Email</option>
                            </select>
                          </div>
                          <!-- ID -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <div class="flex flex-1 items-baseline">
                              <label
                                for="exampleFormControlInput5"
                                class="form-label inline-block mb-2 mr-1 text-gray-700"
                                >Id</label
                              >
                              <svg
                                xmlns="http://www.w3.org/2000/svg"
                                x="0px"
                                y="0px"
                                width="14"
                                height="14"
                                viewBox="0 0 48 48"
                                style="fill: #000000"
                              >
                                <path
                                  fill="#3f51b5"
                                  d="M26.4 24.898L27.309 19H30l-2.123 10H25.5l-1.495-5.938L22.505 29h-2.377L18 19h2.695l.891 6.035L23.02 19h1.959L26.4 24.898zM12.4 24.898L13.309 19H16l-2.123 10H11.5l-1.495-5.938L8.505 29H6.128L4 19h2.695l.891 6.035L9.02 19h1.959L12.4 24.898zM40.4 24.898L41.309 19H44l-2.123 10H39.5l-1.495-5.938L36.505 29h-2.377L32 19h2.695l.891 6.035L37.02 19h1.959L40.4 24.898z"
                                ></path>
                                <path
                                  fill="#2196f3"
                                  d="M8.5 12H39.5V14H8.5z"
                                ></path>
                                <path
                                  fill="#2196f3"
                                  d="M8.5 12H39.5V14H8.5zM8.5 34H39.5V36H8.5z"
                                ></path>
                                <path
                                  fill="#2196f3"
                                  d="M7.426 17c2.085-4.911 6.289-8.714 11.462-10.25-1.965 2.298-3.48 5.898-4.273 10.25h2.048C17.776 11.281 20.182 6.998 23 6.164V17h2V6.164c2.818.833 5.224 5.117 6.337 10.836h2.048c-.793-4.352-2.308-7.952-4.273-10.25 5.173 1.536 9.377 5.339 11.462 10.25h2.137C39.865 9.419 32.563 4 24 4S8.135 9.419 5.289 17H7.426zM40.574 31c-2.085 4.911-6.289 8.714-11.462 10.25 1.965-2.298 3.48-5.898 4.273-10.25h-2.048C30.224 36.719 27.818 41.002 25 41.836V31h-2v10.836c-2.818-.833-5.224-5.117-6.337-10.836h-2.048c.793 4.352 2.308 7.952 4.273 10.25C13.715 39.714 9.511 35.911 7.426 31H5.289C8.135 38.581 15.437 44 24 44s15.865-5.419 18.711-13H40.574z"
                                ></path>
                              </svg>
                            </div>
                            <input
                              type="text"
                              class="form-control md:ml-1.5 flex-[4] block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-gray-100 bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                              id="exampleFormControlInput5"
                              placeholder="DS-180722-1"
                              aria-label="Disabled input example"
                              disabled
                            />
                          </div>
                          <!-- Description -->
                          <div
                            class="md:flex items-baseline justify-between col-span-6 sm:col-span-3"
                          >
                            <label
                              for="description"
                              class="block flex-1 text-sm font-medium text-gray-700"
                            >
                              Description
                            </label>
                            <textarea
                              id="description"
                              name="description"
                              rows="3"
                              class="shadow-sm focus:ring-indigo-500 flex-[4] focus:border-indigo-500 mt-1 block w-full sm:text-sm border border-gray-300 rounded-md"
                              placeholder="Write something here"
                            />
                          </div>
                        </div>
                      </div>
                    </div>
                  </form>
                </div>
                <!-- /End replace -->
              </div>
            </div>
          </main>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import {
  ClockIcon,
  AdjustmentsIcon,
  PencilAltIcon,
  ClipboardCheckIcon,
  TagIcon,
  PaperClipIcon,
  DotsVerticalIcon,
  PlusSmIcon,
  BellIcon,
  ExclamationCircleIcon,
  XIcon,
  MenuIcon,
  ArrowLeftIcon,
  ArrowRightIcon,
} from "@heroicons/vue/outline";
import {
  Dialog,
  DialogPanel,
  Menu,
  MenuButton,
  MenuItem,
  MenuItems,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";

const navigation = [
  { name: "Field", href: "#", icon: PencilAltIcon, current: true },
  { name: "Timeline", href: "#", icon: AdjustmentsIcon, current: false },
  { name: "Resource", href: "#", icon: ClockIcon, current: false },
  { name: "Checklist", href: "#", icon: ClipboardCheckIcon, current: false },
  { name: "Attachments", href: "#", icon: PaperClipIcon, current: false },
  { name: "Reminders", href: "#", icon: BellIcon, current: false },
  { name: "Tags", href: "#", icon: TagIcon, current: false },
];
const items = [
  { name: "Clone", href: "#" },
  { name: "Show Settings", href: "#" },
  { name: "Archive", href: "#" },
  { name: "Delete", href: "#" },
];

const tabs = [
  {
    name: "General",
    href: "#",
    icon: ExclamationCircleIcon,
    current: false,
  },
  { name: "Details", href: "#", icon: ExclamationCircleIcon, current: true },
];

const sidebarOpen = ref(false);

let collapse = ref(true);

const menuCollapse = () => {
  collapse.value = !collapse.value;
};
</script>

<style></style>
