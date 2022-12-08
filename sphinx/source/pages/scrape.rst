==================
Setup
==================

We used the below code to scrape out each operating system used in the attacks data set and their corresponding IP address.

.. code-block:: Python

    import requests
    from bs4 import BeautifulSoup


    def get_ips(ip_str):
        ip_str = ip_str.strip()
        vals = ip_str.split(",")
        prefix = vals[0]
        to_ret = [prefix]
        for ip in vals[1:]:
            to_ret.append(".".join(prefix.split('.')[:-1]+[ip.strip()]))
        return to_ret

    def main():
        r = requests.get("https://www.unb.ca/cic/datasets/ids-2017.html")
        soup = BeautifulSoup(r.text, features="lxml")
        
        items = soup.find_all("li")
        
        
        os_dict = {}
        
        for item in items:
            text = item.get_text()
            if ":" in text:
                parts = text.split(":")
                ips = get_ips(parts[1])
                for ip in ips:
                    os_dict[ip] = parts[0]
        
        print(os_dict)

    if __name__=="__main__":
        main()    
