var DialogUtils = {

		DEFAULT_BTN_CLASS : "btn btn-secondary",
		BTN_DANGER        : "btn btn-danger",
		BTN_DANGER        : "btn btn-success",
		SUCCESS_LABEL     : "Operazione completata",
		ERROR_LABEL       : "Errore",
		LARGE_DIALOG      : "LARGE",
		SMALL_DIALOG      : "SMALL",
		
		backdrop          : 'static',
		keyboard          : true,
		show              : true,
		
		BuildGenericModal : function(title, dialogContentDiv) {
			$('.modal').modal('hide');
			$('#modalGenerico .modal-title').html(title);
			$('#modalGenerico .modal-body').html(dialogContentDiv);
			$('#modalGenerico .modal-footer').html(Templates.footer);
			
			DialogUtils.DefaultSettings();
			
			$('#modalGenerico').modal({
				show     : this.show,
				keyboard : this.keyboard,
				backdrop : this.backdrop,
			}) 
		},
		
		DefaultSettings : function(){
			if($('.modal-dialog').hasClass('modal-lg'))
				$('.modal-dialog').removeClass('modal-lg');
			if($('.modal-dialog').hasClass('modal-sm'))
				$('.modal-dialog').removeClass('modal-sm');
			$('#modalGenerico .modal-footer').html('');
			this.backdrop = 'static';
			this.keyboard = true;
			this.show     = true;
		},

		/**
		 * HOW TO USE: 
		 * 
		 * -DialogUtils.CreateDialog('Title', buttons, options, 'body')  dove buttons e options sono così defniti:
		 * 
		 * var buttons = [{label : 'button 1', btnClass : 'btn btn-success', callback : prova},
		 *		   		  {label : 'button 2',btnClass : 'btn btn-danger',callback : prova2}];
		 * var options = {size : 'LARGE'};
		 *
		 * E' possibile passare come parametri della funzione anche stringhe HTML derivanti
		 * dalla compilazione di template Handlebars
		 * 
		 */
		CreateDialog: function(title, buttons, options, dialogContentDiv) {

			$('.modal').modal('hide');
			DialogUtils.DefaultSettings();
			var htmlButtons="";
			
			if (!Common.isNullOrUndefined(options)) {
				if (!Common.isNullOrUndefined(options.backdrop)) {
					Common.backdrop = options.backdrop;
				}
				if (!Common.isNullOrUndefined(options.keyboard)) {
					Common.keyboard = options.keyboard;
				}
				if (!Common.isNullOrUndefined(options.show)) {
					Common.show = options.show;
				}
				if (!Common.isNullOrUndefined(options.size)) {
					if(options.size===DialogUtils.LARGE_DIALOG)
						$('.modal-dialog').addClass('modal-lg');
					else if(options.size===DialogUtils.SMALL_DIALOG)
						$('.modal-dialog').addClass('modal-sm');
					else
						$('.modal-dialog').addClass('modal-md');
				}
			}

			if (!Common.isNullOrUndefined(buttons))
			{
				if(jQuery.type(buttons) === "string"){
					$('#modalGenerico .modal-footer').html(buttons);
				}else{
					$.each(buttons, function( index, value ) {
						var button = $('<button/>', {
						    id: 'btn'+index,
						    class: !Common.isNullOrUndefined(value.btnClass) ? value.btnClass : DialogUtils.DEFAULT_BTN_CLASS,
						    text: value.label
						})
						if(!Common.isNullOrUndefined(value.callback))
							button.click(value.callback);
						button.appendTo('#modalGenerico .modal-footer');
					});
				}
			}else{
				$('#modalGenerico .modal-footer').html(Templates.footer);
			}

			$('#modalGenerico .modal-title').html(title);
			$('#modalGenerico .modal-body').html(dialogContentDiv);
			$('#modalGenerico').modal({
				show     :  DialogUtils.show,
				keyboard : 	DialogUtils.keyboard,
				backdrop : 	DialogUtils.backdrop,
			}) 
		},
		
		/**
		 * HOW TO USE: 
		 * 
		 * -DialogUtils.CreateDialogError();
		 * -DialogUtils.CreateDialogError('modal body');
		 * 
		 *  E' possibile passare come parametro della funzione anche una stringa HTML derivante
		 *  dalla compilazione di un template Handlebars
		 */
		CreateDialogError: function(dialogContentDiv) {
			if(!Common.isNullOrUndefined(dialogContentDiv))
				DialogUtils.BuildGenericModal(DialogUtils.ERROR_LABEL,dialogContentDiv);
			else
				DialogUtils.BuildGenericModal(DialogUtils.ERROR_LABEL,Templates.esitoOperazioneKO);
		},
		
		/**
		 * HOW TO USE: 
		 * 
		 * -DialogUtils.CreateDialogSuccess();
		 * -DialogUtils.CreateDialogSuccess('modal body');
		 * 
		 * E' possibile passare come parametro della funzione anche una stringa HTML derivante
		 * dalla compilazione di un template Handlebars
		 */
		CreateDialogSuccess: function(dialogContentDiv) {
			if(!Common.isNullOrUndefined(dialogContentDiv))
				DialogUtils.BuildGenericModal(DialogUtils.SUCCESS_LABEL,dialogContentDiv);
			else
				DialogUtils.BuildGenericModal(DialogUtils.SUCCESS_LABEL,Templates.esitoOperazioneOK);
		}
}