>WPF application

···
            var timer = new DispatcherTimer();
            timer.Interval = TimeSpan.FromSeconds(1);
            timer.Tick += new EventHandler(async(object s, EventArgs a) =>
            {
                    this.SetTime();                     
            });
            timer.Start();
···
