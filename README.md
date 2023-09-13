# practice4
for exam
1. Create REST API for add record in the mongodb cloud database using node JS and these APIs will return JSON. When it will add Recor/document, If the emailid is already available in any records/documents in mongodb database then it will NOT INSERT RECORD/document and SHOW ERROR MESSAGE to the user that EMAILID ALREADY REGISTERED. 

Create Image and Push it in the docker hub. Then deploy it in the minikube cluster. Call that API from postman and display result

[Answer2.pdf](https://github.com/Pengsea1/practice4/files/12582104/Answer2.pdf)
[Answer.pdf](https://github.com/Pengsea1/practice4/files/12582105/Answer.pdf)

/*
{
  "eid":500,
  "ename":"Chandan",
  "eemail":"chan@gmail.com",
  "pass":"abc123"
}
*/
app.post('/reg', (req, res) => {
  EmpModel.findOne({ "email": req.params.eemail })
        .then(objarr => {
          console.log(objarr);
            if (objarr.length > 0) {
             return res.status(200).send('Email is already exist');

            }
            else {
              const empobj = new EmpModel({
                empid: req.body.eid,
                name: req.body.ename,
                email: req.body.eemail,
                password: req.body.pass,
              }); 
            
              empobj.save()
                  .then(eobj => {
                      res.status(200).send('DOCUMENT INSERED');
                  })//CLOSE THEN
                  .catch(err => {
                      res.status(500).send({ message: err.message || 'Error in Employee Save ' })
                  });//CLOSE CATCH
            }
        }) //CLOSE THEN
        .catch(err => {
            return res.status(500).send({ message: "DB Problem..Error in Retriving with id " + req.params.empid });
        })//CLOSE CATCH
   
}//CLOSE CALLBACK FUNCTION BODY
);//CLOSE POST METHOD
