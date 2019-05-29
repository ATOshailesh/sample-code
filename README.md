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
	 
## Get time ago
	func getTimesAgo(_ date : Date) -> String {
        
        let units = Set<Calendar.Component>([.year, .month, .day, .weekOfYear])
        let components = Calendar.current.dateComponents(units, from: date as Date, to: Date())
        
        if components.year! > 0 {
            return ("\(components.year!)y ago")
            
        } else if components.month! > 0 {
            return ("\(components.month!)m ago")
            
        } else if components.weekOfYear! > 0 {
            return ("\(components.weekOfYear!)w ago")
            
        } else if (components.day! > 0) {
            return ("\(components.day!)d ago")
            
        } else {
            return "To day"
        }
	}
	
### NSDictionary key validation
	func KeyValidationFor_String(strKey: String) -> String {
		
		// CHECK FOR EMPTY
		if(self.allKeys.count == 0) {
			return String()
		}
		
		// CHECK IF KEY EXIST
		if let val = self.object(forKey: strKey) {
			if((val as AnyObject).isEqual(NSNull())) {
				return String()
			}
		} else {
			// KEY NOT FOUND
			return String()
		}
		
		// CHECK FOR NIL VALUE
		let aValue : AnyObject = self.object(forKey: strKey)! as AnyObject
		if aValue.isEqual(NSNull()) {
			return String()
		}
		else if(aValue.isKind(of: NSNumber.self)){
			return String(format:"%f", (aValue as! NSNumber).doubleValue)
		}
		else {
			
			if aValue is String {
				return self.object(forKey: strKey) as! String
			}
			else{
				return String()
			}
		}
	}

## Should Change Charater
	let set = CharacterSet(charactersIn: "ABCDEFGHIJKLMONPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz ").inverted
	let set = CharacterSet(charactersIn: "@_.abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890-").inverted
            if string.rangeOfCharacter(from: set) != nil{
                shouldReturn = false
            }

## Dictionary To String
	let body = ["userName":"shailesh","userImage":"myimage.jpg"]
        var jsonData = Data()
        do {
            jsonData = try JSONSerialization.data(withJSONObject: body, options: .prettyPrinted)
            // here "jsonData" is the dictionary encoded in JSON data
        }catch {
            print(error.localizedDescription)
        }
        let vStr = String(data: jsonData, encoding: String.Encoding.utf8) ?? ""
