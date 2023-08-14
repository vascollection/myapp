<!-- filter buttons, completed count, check all button, remove completed, UI -->

<script>
  import { v4 as uuidv4 } from "uuid"

  let inputValue = ""
  let status = "active"

  let completCount = 0
  let outputList = []

  let indexToEdit
  let taskToEdit
  let newTask
  let editCondition = false
  let isChecked = false
  let changeStatus = []

  let activeTabFilter = "all"


  //ADD BUTTON
  const addToList = () => {
    if (inputValue.trim() !== "") {
      outputList = [...outputList, { id: uuidv4(), task: inputValue, status: status }]
      inputValue = ""
    }
  }

  // TARGET ID
  const targetID = (item) => {
    let taskObjectID = outputList.find((i) => item === i.id)
    return taskObjectID
  }

  //REITERATE ARRAY | COMPLETED COUNTER
  const reiterateArray = (x) => {
    outputList = [...x]
    completCount = outputList.filter(outputList => outputList.status === "completed").length
  }

  //DELETE BUTTON
  const removeToList = (taskIndex) => {
    if (taskIndex !== -1) {
      outputList.splice(taskIndex, 1)
      reiterateArray(outputList)
    }
  }

  //EDIT BUTTON
  const editItem = (id, index) => {
    indexToEdit = index
    taskToEdit = targetID(id)
    editCondition = true
  }

  //SAVE EDIT BUTTON
  const editTask = () => {
    if (newTask.trim() !== "") {
      taskToEdit = {
        id: taskToEdit.id,
        task: newTask,
        status: taskToEdit.status,
      }
      outputList.splice(indexToEdit, 1, taskToEdit)
      reiterateArray(outputList)
    }
    taskToEdit = ""
    newTask = ""
    editCondition = false
  }

  //COMPLETE TASK
  const checkBox = (id) => {
    isChecked = !isChecked
    changeStatus = outputList.find((item) => id === item.id)

    if (changeStatus.status === "completed") {
      changeStatus.status = changeStatus.status = "active"
      completCount -= 1
    } else {
      changeStatus.status = changeStatus.status = "completed"
      completCount += 1
    }
    outputList.splice(changeStatus.index, changeStatus)
    reiterateArray(outputList)
  }

  // CANCEL EDIT BUTTON
  const cancelEdit = () => {
    editCondition = false
    newTask = ""
  }

  // FILTER
  const activeTab = (tab) => {
    activeTabFilter = tab
  }

  //TABS FILTER
  const CheckStatus = (item) => {
    if (targetID(item).status === "active" && activeTabFilter === "active") {
      return "active"
    } else if (targetID(item).status === "completed" && activeTabFilter === "completed") {
      return "completed"
    } else {
      return "all"
    }
  }

  //CHECK ALL BUTTON
  const checkAll = () => {
    if (outputList.length != 0){
      let tempArray = outputList.filter( outputList => outputList.status === "active")
        for (let i = 0; i <= tempArray.length; i++){
          console.log(tempArray[i].id);
          let change = tempArray[i].status = "comepleted"
          // for(let j =0; j <=  tempArray[i].length; j++){
            console.log(change);
          }
          
          
      
      
      

        // if(arrayIndex ){
        //  statusToChange.status = "completed"
        //  reiterateArray(x)
        //  console.log(outputList);
        // }
      // }
    }
  }
  
</script>

<!--------------------------------------------------->
<div class="container">
  <!-- HEADER -->
  <div class="heading">
    <h1>to do</h1>
    <p>web application using Svelte</p>
  </div>

  <!-- INPUT -->
  {#if editCondition}
    <div class="input">
      <input type="text" bind:value={newTask} />
      <div class="action">
        <button class="buttonStyle" on:click={editTask} type="button">Save</button>
        <button class="buttonStyle" on:click={cancelEdit}>Cancel</button>
      </div>
    </div>
  {:else}
    <div class="input">
      <input type="text" placeholder="Enter a task here" bind:value={inputValue} />
      <button class="buttonStyle" on:click={addToList} type="button">Add task</button>
    </div>
  {/if}

  <!-- OUTPUT -->
  <div class="output">
    <div class="heading">
      <buttonGroup class="buttonGroup">
        <button class:selected={activeTabFilter === "all"} id="all" on:click={() => activeTab("all")}>All</button>
        <button class:selected={activeTabFilter === "active"} id="active" on:click={() => activeTab("active")}
          >Active</button
        >
        <button class:selected={activeTabFilter === "completed"} id="completed" on:click={() => activeTab("completed")}
          >Completed</button
        >
      </buttonGroup>
      <p>Task Completed: {completCount}</p>
    </div>
    <table>
      <tbody>
        {#if outputList.length != 0}
          {#each outputList as item, index (item)}
            {#if CheckStatus(item.id) === activeTabFilter}
              <tr class:checked={item.status === "completed"}>
                <td><input type="checkbox" on:change={() => checkBox(item.id)} checked={item.status === "active" ? false : true} /></td>
                <td class="taskContent">{item.task}</td>
                <td class="col2">
                  <button class="editHover" on:click={() => editItem(item.id, index) }
                    ><svg xmlns="http://www.w3.org/2000/svg" width="18" height="19" viewBox="0 0 18 19" fill="none">
                      <path
                        d="M12.6463 3.865L13.9119 2.59938C14.4611 2.05021 15.3514 2.05021 15.9006 2.59938C16.4498 3.14856 16.4498 4.03894 15.9006 4.58812L7.93664 12.5521C7.54013 12.9486 7.05108 13.2401 6.51367 13.4002L4.5 14L5.09984 11.9863C5.25992 11.4489 5.55138 10.9599 5.94789 10.5634L12.6463 3.865ZM12.6463 3.865L14.625 5.84374M13.5 11V14.5625C13.5 15.4945 12.7445 16.25 11.8125 16.25H3.9375C3.00552 16.25 2.25 15.4945 2.25 14.5625V6.68749C2.25 5.75551 3.00552 4.99999 3.9375 4.99999H7.5"
                        stroke="#737373"
                        stroke-width="1.25"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                    </svg></button
                  >
                  <button class="deleteHover" on:click={() => removeToList(index)}
                    ><svg xmlns="http://www.w3.org/2000/svg" width="18" height="19" viewBox="0 0 18 19" fill="none">
                      <path
                        d="M11.0553 7.25L10.7957 14M7.20433 14L6.94471 7.25M14.4207 4.84293C14.6772 4.88166 14.9328 4.9231 15.1875 4.96722M14.4207 4.84293L13.6199 15.2544C13.5522 16.1336 12.8191 16.8125 11.9373 16.8125H6.06268C5.1809 16.8125 4.44778 16.1336 4.38015 15.2544L3.57926 4.84293M14.4207 4.84293C13.5609 4.71308 12.6911 4.61364 11.8125 4.54574M2.8125 4.96722C3.06718 4.9231 3.32278 4.88166 3.57926 4.84293M3.57926 4.84293C4.43908 4.71308 5.30887 4.61364 6.1875 4.54574M11.8125 4.54574V3.85864C11.8125 2.97412 11.1295 2.23567 10.2454 2.20739C9.83192 2.19416 9.41674 2.1875 9 2.1875C8.58326 2.1875 8.16808 2.19416 7.75456 2.20739C6.87049 2.23567 6.1875 2.97412 6.1875 3.85864V4.54574M11.8125 4.54574C10.8844 4.47403 9.94648 4.4375 9 4.4375C8.05352 4.4375 7.11558 4.47403 6.1875 4.54574"
                        stroke="#737373"
                        stroke-width="1.25"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                    </svg></button
                  >
                </td>
              </tr>
            {:else}
              {#if activeTabFilter === "active"}
              <tr>
                <td>No active task available</td>
              </tr>
              {:else}
              <tr>
                <td>No task completed</td>
              </tr>
              {/if}
            {/if}
          {/each}
        {:else}
          <tr>
            <td>No task available</td>
          </tr>
        {/if}
      </tbody>
    </table>
    <buttonGroup class="finish">
        <button on:click={checkAll} >Check all</button>
        <button>Remove completed</button>
    </buttonGroup>
  </div>
</div>
