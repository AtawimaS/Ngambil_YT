# Ngambil_YT
## How to Use
link_youtube = ['https://www.youtube.com/watch?v=WWbyYFPHDH8&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=10',
                'https://www.youtube.com/watch?v=U30sF4m0bd0&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=11',
                'https://www.youtube.com/watch?v=f0a1XXmaQp8&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=12', 
                'https://www.youtube.com/watch?v=oe7DW4rSH1o&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=13',
                'https://www.youtube.com/watch?v=Sj1ybuDDf9I&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=14',
                'https://www.youtube.com/watch?v=z69XYXpvVrE&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=15',
                'https://www.youtube.com/watch?v=5wwXKtLkyqs&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=16',
                'https://www.youtube.com/watch?v=yKovaQ6tyV8&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=17']


all_comments = pd.DataFrame(columns=["URL", "Comment"])
for url in link_youtube:
    try:
        # Ambil 25 komentar dari setiap video
        comments = ngambil_youtube(url, 25)
        
        # Buat DataFrame sementara untuk setiap URL
        df_temp = pd.DataFrame({"URL": url, "Comment": comments})
        
        # Tambahkan DataFrame sementara ke DataFrame utama
        all_comments = pd.concat([all_comments, df_temp], ignore_index=True)
        print(url, " Done")
        
    except Exception as e:
        print(f"Error pada URL {url}: {e}")
        continue
