# pseudo-code

// CASE 1: Employee Submits Request

    SubmitVacationRequest(employee, dates, category){
        IF (ValidateRules(employee, dates, category) == FALSE)
            RETURN "Error: Request does not pass rules."
        
        newRequest = CreateRequest(employee, dates, category, vacation_status_id="PENDING")
        SaveToDatabase(newRequest)
        
        IF (employee.RequiresManagerApproval == TRUE)
            SendEmail(to=employee.Manager, subject="Approval Needed", link=newRequest.Request_id)
        ELSE{
            // High-level employees skip manager approval
            newRequest.vacation_status_id = "HR_PENDING"
            SaveToDatabase(newRequest)
            SendEmail(to="HR_Team", subject="HR Validation Needed")
            }
    }
  

// CASE 2: Manager/HR Approves 

    ProcessApproval(request_id, actor, decision){
        request = GetRequestFromDatabase(request_id)
        
        IF (actor.role == "MANAGER" AND request.vacation_status_id == "PENDING"){
            IF (decision == "APPROVE"){
                request.vacation_status_id = "HR_PENDING" // NEW STATE
                SendEmail(to="HR_Team", subject="Request needs HR Validation")
                }
            ELSE IF (decision == "DENY"){
                request.vacation_status_id = "REJECTED"
                SendEmail(to=request.Employee_id, subject="Request Rejected")
                }
          }
                
        ELSE IF (actor.role == "HR_CLERK" AND request.vacation_status_id == "HR_PENDING"){
            IF (decision == "APPROVE"){
                request.vacation_status_id = "HR_APPROVAL" 
                SendEmail(to=request.Employee_id, subject="Request Fully Approved")
                }
            ELSE IF (decision == "DENY"){
                request.vacation_status_id = "REJECTED"
                SendEmail(to=request.Employee_id, subject="Request Rejected by HR")
                }
          }
            
        SaveToDatabase(request)
      }


      <img width="1164" height="648" alt="image" src="https://github.com/user-attachments/assets/ff88d0d1-8b56-4145-85b0-f56cd1d4ab10" />
