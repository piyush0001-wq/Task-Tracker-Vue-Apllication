<template>
    <div>
        <form @submit="onSubmit" class="add-form">
            <div class="form-control">
                <label >Task</label>
                <input type="text" v-model="text" name="text" placeholder="Add Task" required/>
            </div>
            <!-- {{text}} -->
            <div class="form-control">
                <label >Day & Time</label>
                <input type="text" v-model="day" name="day" placeholder="Add Day & Time" required />
            </div>
            <div class="form-control form-control-check">
                <label >Set Reminder</label>
                <input type="checkbox" v-model="reminder" name="reminder"  />
            </div>
            <input type="submit" value="Save Task" class="btn btn-block" />
        </form>
    </div>
</template>
<script>
export default{
    name:'AddTask',
    data(){
        return{
            text:'',
            day:'',
            reminder: false
        }
    },
    methods: {
        onSubmit(e){
            e.preventDefault()

            
            const newTask = {
               
                text : this.text,
                day : this.day,
                reminder: this.reminder,
            }

            // console.log(newTask)
            this.$emit('add-task', newTask)

            this.text = ''
            this.day = ''
            this.reminder = false
            
        }
    },
}
</script>

<style scoped>
::placeholder{
    color: rgb(173, 173, 173);
    
}
.add-form{
    margin-top: 20px;
    margin-bottom: 20px;

}
.form-control{
    margin-top: 20px ;
    /* margin-bottom: 20px ; */
}
.form-control label{
    display: block;
    font-weight: 700;
}
.form-control input:not([type=checkbox]){
    width: 100%;
    height: 40px;
    padding: 10px;
    font-size: 20px;
    background: rgb(255, 255, 255);
    outline: none;
    border: none;
    border-bottom: 2px solid;
}
 
.form-control-check{
   display: flex;
   justify-content: space-around;
   align-items: center;
   margin-bottom: 20px;
}
.btn-block{
    width: 100%;

}
</style>