# Sample-code
For get information.

## Convert server date to Display Date
	struct DateFormet {
		static let serverDateFormate = "yyyy-MM-dd HH:mm:ss"
		static let dateFormet 	     = "yyyy-MM-dd"
  	}
	
 	 func convertServerDateToDisplayDateFormet() -> String {
		let dateformet = DateFormatter()
    		dateformet.dateFormat = DateFormet.serverDateFormate
    		if(dateformet.date(from: self) != nil){
			let date = dateformet.date(from: self)
        		dateformet.dateFormat = DateFormet.dateFormet
        		return dateformet.string(from: date!)
     	}
     	return self
 	 }

