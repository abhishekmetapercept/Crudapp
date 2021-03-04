<!DOCTYPE html>
<html>
<head>
    <title>CRUD Application using JavaScript</title>
    <style>
        table 
        {
            width: 100%;
            font: 17px Calibri;
        }
        table, th, td 
        {
            border: solid 1px #DDD;
            border-collapse: collapse;
            padding: 2px 3px;
            text-align: center;
        }
      
        input[type='button'] 
        {
            font: 15px Calibri;
            cursor: pointer;
            border: none;
            color: #FFF;
        }
        
        input[type='text'], select 
        {
            font: 17px Calibri;
            text-align: center;
            border: solid 1px #CCC;
            width: auto;
            padding: 2px 3px;
        }
    </style>
</head>
<body>
    <div id="container" style="width:700px;">
    </div>
</body>

<script>
    var crudApp = new function () {

        // AN ARRAY OF JSON OBJECTS WITH VALUES.
        this.myBooks = [
            { ID: '1', Book_Name: 'Computer Architecture', Category: 'Computers', Price: 125.60 },
            { ID: '2', Book_Name: 'Asp.Net 4 Blue Book', Category: 'Programming', Price: 56.00 },
            { ID: '3', Book_Name: 'Popular Science', Category: 'Science', Price: 210.40 }
        ]

        this.category = ['Business', 'Computers', 'Programming', 'Science'];
        this.col = [];

        this.createTable = function () {

            // EXTRACT VALUE FOR TABLE HEADER.
            for (var i = 0; i < this.myBooks.length; i++) {
                for (var key in this.myBooks[i]) {
                    if (this.col.indexOf(key) === -1) {
                        this.col.push(key);
                    }
                }
            }

            // CREATE A TABLE.
            var table = document.createElement('table');
            table.setAttribute('id', 'booksTable');     

            var tr = table.insertRow(-1);               

            for (var h = 0; h < this.col.length; h++) {
                // ADD TABLE HEADER.
                var th = document.createElement('th');
                th.innerHTML = this.col[h].replace('_', ' ');
                tr.appendChild(th);
            }

            // ADD ROWS USING JSON DATA.
            for (var i = 0; i < this.myBooks.length; i++) {

                tr = table.insertRow(-1);       

                for (var j = 0; j < this.col.length; j++) {
                    var tabCell = tr.insertCell(-1);
                    tabCell.innerHTML = this.myBooks[i][this.col[j]];
                }

                // DYNAMICALLY CREATE AND ADD ELEMENTS TO TABLE CELLS WITH EVENTS.

                this.td = document.createElement('td');

                // *** CANCEL OPTION.
                tr.appendChild(this.td);
                var lblCancel = document.createElement('label');
                lblCancel.innerHTML = '✖';
                lblCancel.setAttribute('onclick', 'crudApp.Cancel(this)');
                lblCancel.setAttribute('style', 'display:none;');
                lblCancel.setAttribute('title', 'Cancel');
                lblCancel.setAttribute('id', 'lbl' + i);
                this.td.appendChild(lblCancel);

                // *** SAVE.
                tr.appendChild(this.td);
                var btSave = document.createElement('input');

                btSave.setAttribute('type', 'button');    
                btSave.setAttribute('value', 'Save');
                btSave.setAttribute('id', 'Save' + i);
                btSave.setAttribute('style', 'display:none;');
                btSave.setAttribute('onclick', 'crudApp.Save(this)');      
                this.td.appendChild(btSave);

                // *** UPDATE.
                tr.appendChild(this.td);
                var btUpdate = document.createElement('input');

                btUpdate.setAttribute('type', 'button');   
                btUpdate.setAttribute('value', 'Update');
                btUpdate.setAttribute('id', 'Edit' + i);
                btUpdate.setAttribute('style', 'background-color:#44CCEB;');
                btUpdate.setAttribute('onclick', 'crudApp.Update(this)');   
                this.td.appendChild(btUpdate);

                // *** DELETE.
                this.td = document.createElement('th');
                tr.appendChild(this.td);
                var btDelete = document.createElement('input');
                btDelete.setAttribute('type', 'button');    
                btDelete.setAttribute('value', 'Delete');
                btDelete.setAttribute('style', 'background-color:#ED5650;');
                btDelete.setAttribute('onclick', 'crudApp.Delete(this)');   
                this.td.appendChild(btDelete);
            }



            tr = table.insertRow(-1);         

            for (var j = 0; j < this.col.length; j++) {
                var newCell = tr.insertCell(-1);
                if (j >= 1) {

                    if (j == 2) {   

                        var select = document.createElement('select');     
                        select.innerHTML = '<option value=""></option>';
                        for (k = 0; k < this.category.length; k++) {
                            select.innerHTML = select.innerHTML +
                                '<option value="' + this.category[k] + '">' + this.category[k] + '</option>';
                        }
                        newCell.appendChild(select);
                    }
                    else {
                        var tBox = document.createElement('input');    
                        tBox.setAttribute('type', 'text');
                        tBox.setAttribute('value', '');
                        newCell.appendChild(tBox);
                    }
                }
            }

            this.td = document.createElement('td');
            tr.appendChild(this.td);

            var btNew = document.createElement('input');

            btNew.setAttribute('type', 'button');      
            btNew.setAttribute('value', 'Create');
            btNew.setAttribute('id', 'New' + i);
            btNew.setAttribute('style', 'background-color:#207DD1;');
            btNew.setAttribute('onclick', 'crudApp.CreateNew(this)');    
            this.td.appendChild(btNew);

            var div = document.getElementById('container');
            div.innerHTML = '';
            div.appendChild(table);  
        };

        this.Cancel = function (oButton) {

            oButton.setAttribute('style', 'display:none; float:none;');

            var activeRow = oButton.parentNode.parentNode.rowIndex;

            var btSave = document.getElementById('Save' + (activeRow - 1));
            btSave.setAttribute('style', 'display:none;');

            var btUpdate = document.getElementById('Edit' + (activeRow - 1));
            btUpdate.setAttribute('style', 'display:block; margin:0 auto; background-color:#44CCEB;');

            var tab = document.getElementById('booksTable').rows[activeRow];

            for (i = 0; i < this.col.length; i++) {
                var td = tab.getElementsByTagName("td")[i];
                td.innerHTML = this.myBooks[(activeRow - 1)][this.col[i]];
            }
        }


        this.Update = function (oButton) {
            var activeRow = oButton.parentNode.parentNode.rowIndex;
            var tab = document.getElementById('booksTable').rows[activeRow];

            for (i = 1; i < 4; i++) {
                if (i == 2) {
                    var td = tab.getElementsByTagName("td")[i];
                    var ele = document.createElement('select');    
                    ele.innerHTML = '<option value="' + td.innerText + '">' + td.innerText + '</option>';
                    for (k = 0; k < this.category.length; k++) {
                        ele.innerHTML = ele.innerHTML +
                            '<option value="' + this.category[k] + '">' + this.category[k] + '</option>';
                    }
                    td.innerText = '';
                    td.appendChild(ele);
                }
                else {
                    var td = tab.getElementsByTagName("td")[i];
                    var ele = document.createElement('input');    
                    ele.setAttribute('type', 'text');
                    ele.setAttribute('value', td.innerText);
                    td.innerText = '';
                    td.appendChild(ele);
                }
            }

            var lblCancel = document.getElementById('lbl' + (activeRow - 1));
            lblCancel.setAttribute('style', 'cursor:pointer; display:block; width:20px; float:left; position: absolute;');

            var btSave = document.getElementById('Save' + (activeRow - 1));
            btSave.setAttribute('style', 'display:block; margin-left:30px; float:left; background-color:#2DBF64;');


            oButton.setAttribute('style', 'display:none;');
        };


        this.Delete = function (oButton) {
            var activeRow = oButton.parentNode.parentNode.rowIndex;
            this.myBooks.splice((activeRow - 1), 1);    
            this.createTable();                      
        };

        this.Save = function (oButton) {
            var activeRow = oButton.parentNode.parentNode.rowIndex;
            var tab = document.getElementById('booksTable').rows[activeRow];

            for (i = 1; i < this.col.length; i++) {
                var td = tab.getElementsByTagName("td")[i];
                if (td.childNodes[0].getAttribute('type') == 'text' || td.childNodes[0].tagName == 'SELECT') {  
                    this.myBooks[(activeRow - 1)][this.col[i]] = td.childNodes[0].value;      
                }
            }
            this.createTable();     
        }
        this.CreateNew = function (oButton) {
            var activeRow = oButton.parentNode.parentNode.rowIndex;
            var tab = document.getElementById('booksTable').rows[activeRow];
            var obj = {};

            for (i = 1; i < this.col.length; i++) {
                var td = tab.getElementsByTagName("td")[i];
                if (td.childNodes[0].getAttribute('type') == 'text' || td.childNodes[0].tagName == 'SELECT') {     
                    var txtVal = td.childNodes[0].value;
                    if (txtVal != '') {
                        obj[this.col[i]] = txtVal.trim();
                    }
                    else {
                        obj = '';
                        alert('all fields are compulsory');
                        break;
                    }
                }
            }
            obj[this.col[0]] = this.myBooks.length + 1;    

            if (Object.keys(obj).length > 0) {      
                this.myBooks.push(obj);           
                this.createTable();                 
            }
        }
    }

    crudApp.createTable();
</script>
</html>
