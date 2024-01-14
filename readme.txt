Clen Code ทำให้ code อ่านง่าย ประสิทธิภาพเพิ่มขึ้น

1. Variable
    อ่านเข้าใจ, รวมกลุ่มถ้าประเภทเดียวกัน, ไม่ย่อ, สร้างตัวแปรและที่มาของเลขไม่ใส่เลขเดี่ยวๆ, object ไม่ตั้งชื่อซ้ำซ้อน
2. Functions
    ใช้defaultแทนshortcutcondition, parameterเยอะรับobjectแทน, 2functionค่าใกล้เคียงกันให้รวมเหลือfunctionเดียวและแยกการทำงานส่วนที่ต่างเป็นfunctionใหม่, 
    function ควรทำแค่สิ่งเดียว, setdefaultobjectsด้วยobject.assign, ไม่ใช้parameterเช็คconditionให้แยกfunctionแทน, ห้ามให้ตัวแปรเก่ากระทบสามารถแก้ไขโดยส่งค่าปัจจุบันทางparameter,
    ห้ามให้objectเก่ากระทบสามารถแก้ไขโดยส่งค่าปัจจุบันทางparameterและ...object,  
3. Condurrency (callback, promise, async-await)
    ใช้ แบบเดียวกันทั้ง code หากใช้ async-await ควรใส่ try catch
4. Error Handling 
    promise เพิ่ม .catch(), async-await ควรใส่ try catch()
5. อื่นๆ

1. Variable

    -ตั้งชื่อตัวแปร ให้อ่านเข้าใจได้

        ex. // Good

            let currentDate = new Date(); // ไม่ควรใช้ let cd  = new Date()
            let userName = 'JohnDoe'; // ไม่ควรใช้ let data = 'JohnDoe'
            let maxScore = 42; // ไม่ควรใช้ let number = 42

    -ตั้งชื่อรวมกลุ่มคำศัพท์ประเภทเดียวกัน

        ex. // Bad

            let userSettings = {theme: 'dark', language: 'English'}
            let userProfilePreferences = {theme: 'Light', fontSize: 'medium'}
        
        ex. // Good จะเห็นได้ว่ามีการ setting เหมือนกัน

            let userSettings = {theme: 'dark', language: 'English'}
            let displaySetting = {theme: 'Light', fontSize: 'medium'}

    -อย่าใช้ค่าที่อธิบายไม่ได้ เช่น ค่า 5640000 ไม่รู้ที่มา

        ex. // Bad

            setTimeOut(updateFunction, 5640000);

        ex. // Good
 
            const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //8640000;
            setTimeOut(blastOff, MILLISECONDS_PER_DAY);

    -ไม่่ย่อชื่อ

        ex. // Bad 

            let loc = ['Austin', 'New York', 'San Francisco']

            for (let i = 0; i < loc.length; i++){
                // process loc[i]
            }
            loc.forEach(l => {
                // ไม่ทราบค่า l
            })

        ex. // Good

            let cities = ['Austin', 'New York', 'San Francisco'];

            for (let cityIndex = 0; cityIndex < cities.length; cityIndex ++){
                // process[cityIndex]
            }
            cities.forEach(city => {
                // city คือชื่อเมืองที่กำลัง loop
            })

    -class/object ถ้าตัวเองสื่อความหมายอยู่แล้วไม่ต้องใส่ซ้ำซ้อน

        ex. // Bad
            const Student = {
                studentName : 'Mike',
                studentAge : 24,
                studentGrade: 3
            }

        ex. // Good

            const Student = {
                name : 'Mike',
                age: 24,
                grade: 3 
            }

2. Functions

    -ใช้ default แทน shortcut condition
        ex. // Bad
            function createGreeting(name, timeOfDay){
                name = name | 'Guest';
                timeOfDay = timeOfDay || 'Day';
                return 'Hello ${name}, $[timeOfDay]'
            }

        ex. // Good
            function createGreeting(name = 'Game', timeOfDay = 'Day'){
                return 'Hello ${name}, $[timeOfDay]'
            }


    -หาก function มีการรับค่า parameter เยอะรับเป็น object แทน

        ex. // Bad  

            function createUser(firstName, lastName, age, email, phoneNumber, address){

            }
        
        ex. // Good

            แบบที่ 1
                function createUser(name, contactDetails){

                }

                createUser(
                    {firstName: 'John', lastName: 'Doe'},
                    {age:30, email: 'john@example.com', phoneNumber:'1234', address: 'saimai'}
                )

            แบบที่ 2
                function createUser({firstName, lastName}, {age, email, phoneNumber, address}){

                }

                createUser(
                    {firstName: 'John', lastName: 'Doe'},
                    {age:30, email: 'john@example.com', phoneNumber:'1234', address: 'saimai'}
                )

    -หาก 2 function ซำ้กันเกือบหมดยกเว้นบางส่วน
    ให้นำส่วนที่ต่างทั้ง 2 ไปสร้าง function ใหม่ 2 function
    จากนั้น function เก่าทำการรับ parameter เป็นค่า function ใหม่มาใช้  

        ex. // Bad

            function updateUserProfile(userId, newProfileData){
                let user = database.fetchUserById(userId);
                user.profile = newProfileData;
                database.saveUser(user);
            }

            function updateUserEmailProferences(userId, newEmailPreferences){
                let user = database.fetchUserById(uesrId); // fetching user
                user.emailPreferences = newEmailPreferences;
                database.saveUser(user);

            }

        ex. // Good

            function updateUser(userId, updateFunction){
                let user = database.fetchUserById(userId);
                updateFunction(user);
                database.saveUser(user);
            }

            function updateUserProfile(userId, newProfileData){
                updateUser(userId, user => {
                    user.profile = newProfileData;
                })
            }

            function updateUserEmailPreferences(userId, newEmailPreferences){
                updateUser(userId, user => {
                    user.emailPreferences = newEmailPreferences
                })
            }

    -function ควรทำเพียงอย่างเดียว

        ex. // Bad

            function processAndSaveUserData(inputData){
                // ต้องแปลง data
                let data = JSON.parse(inputData);

                //ต้อง validate data
                if(!data.name || !data.email){
                    throw new Error('Invalid data - name and email are required');
                }

                // ต้องบันทึก data
                database.save(data)
            }

        ex. // Good

            function parseUserData(inputData){
                return JSON.parse(inputData);
            }

            function validateUserData(data){
                if(!data.name || !data.email){
                    throw new Error('Invalid data - name and email are required');
                }
            }

            function saveUserData(data){
                database.save(data)
            }

            function processAndSaveUserData(inputData){
                let data = parseUserData(inputData);
                validateUserData(data);
                saveUserData(data);
            }

    -set default objects ด้วย object.assign

        ex. // Bad

            function createReport(data, options){
                options = options || {}
                let report = {
                    title: options.title || 'Default title',
                    pageSize: options.pageSize || 'A4',
                    format: options.format || 'PDF',
                    headerColor: options.headerColor || '#000',
                };
            }

        ex. // Good

            function createReport(data, options = {}){
                const defaultOptions = {
                    title: 'Default Title',
                    pageSize: 'A4',
                    format: 'PDF',
                    headerColor: '#000',
                };
                options = Object.assign({}, defaultOptions, options);
            }

    -ไม่ใช้ parameter check condition

        ex. // Bad

            function updateDatabaseEntry(id, data, isDelete){
                if(isDelete){
                    database.deleteEntry(id);
                }else{
                    database.updateEntry(id, data);
                }
            }

        ex. // Good

            function updateDatabaseEntry(id, data){
                database.updateEntry(id, data);
            }

            function deleteDatabaseEntry(id){
                database.deleteEntry(id);
            }

    -ห้ามให้ตัวแปรเก่ากระทบ

        แบบที่ 1 สำหรับตัวแปรธรรมดาให้ส่งไปกับparameter

            ex. // Bad

                let userAge = 30;

                function increaseAge(newAge){
                    userAge = newAge;
                }

                increaseAge(31);
                console.lg(userAge); จะเห็นว่าค่า userAge เปลี่ยน

            ex. // Good
        
                function increaseAge(currentAge, increment){
                    return currentAge + increment;
                }

                let userAge = 30;
                let newUserAge = getIncreatedAge(userAge, 1);

        แบบที่ 2 สำหรับ object ให้ ...object

            ex. // Bad
                
                const addItemToCart = (cart, item) => {
                    cart.push({item, date: Date.now()});
                }

            ex. // Good

                const addItemToCart = (cart, item) => {
                    return [...cart, {item, date: Date.now()}];
                }

อธิบาย call back function

    function success() {
        console.log("success called");
    }

    function hello(cb) {
        console.log("hello called");
        cb();
    }

    hello(success); // success เป็น (callback function) เพราะถูกรับและเรียกใช้ใน function hello


อธิบาย promise, async-await (ทำหน้าที่รอ task แต่ละตัวให้เสร็จก่อน)

    function taskOne() {
        setTimeout(function () {
            console.log("this is task 1");
        }, 500);
    } 

    function taskTwo() {
        le.log("this is task 2");
    }

    function taskThree() {
        setTimeout(function() {
            console.log("this is task 3");
        },  1000)
    }
    
    taskOne();
    taskTwo();
    taskThree();

    // output
    this is task 2
    this is task 1
    this is task 3

    เราต้องการให้เรียง task1, task2, task3 แก้โดย promise, async await

    promise อ่านยาก

        taskOne().then(function () {
            taskTwo().then(function () {
                taskThree().then(function () {
                taskFour().then(function () {
                    taskFive().then(function () {
                    taskSix().then(function () {
                        taskSeven().then(function () {
                        taskEight().then(function () {
                            taskNine().then(function () {
                            taskTen();
                            })
                        })
                        })
                    })
                    })
                })
                })
            })
        });

    async-await

        await เพื่อบอกว่าทำ task นี้ให้เสร็จก่อนค่อยทำอันต่อไป

        async function main() {
            await taskOne();
            await taskTwo();
            await taskThree();
            await taskFour();
            await taskFive();
            await taskSix();
            await taskSeven();
            await taskEight();
            await taskNine();
            await taskTen();
        }
        main();



